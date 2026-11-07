# Two false claims, one accepted batch: a Fiat-Shamir contract gap in Jolt's batch openings

*a16z/jolt [#1864](https://github.com/a16z/jolt/issues/1864), resolved by [#1937](https://github.com/a16z/jolt/pull/1937). September 2026.*

This is the story of a bug that turned out not to be exploitable in the zkVM, and why it was still worth reporting. The short version: a public batch opening API promised in its docs that it binds the commitments to the transcript, the code did not, and through that API alone I could get the verifier to accept two false claims. The maintainers decided the docs were wrong, not the code, and rewrote the contract.

## Background: batching openings with a random linear combination

A polynomial commitment scheme lets a prover commit to a polynomial `p` and later prove that `p(z) = v` at some point `z`. When there are many such claims at the same point, opening them one by one is expensive, so the usual trick is a random linear combination (RLC):

1. The verifier picks a random `γ`.
2. Both sides combine the claims: `C = C₁ + γ·C₂ + …` for the commitments and `v = v₁ + γ·v₂ + …` for the values.
3. The prover opens the single combined commitment `C` at `z` and shows it equals `v`.

If any individual claim is false, the combined claim is false with overwhelming probability, because `γ` was chosen after the prover was locked in. In a non-interactive proof, "the verifier picks `γ`" becomes "`γ` is a hash of the transcript so far" (Fiat-Shamir). That only works if everything the prover could still change goes into the hash first. If something is left out, the prover can compute `γ` first and then choose the part that was left out.

## The invariant the code promised

Jolt's `jolt-openings` crate exposes this as `BatchOpeningScheme`, implemented for Dory by `HomomorphicBatch`. The trait's documentation said:

> `Statement` is the public input both sides agree on and bind to the transcript: the opening claims plus the commitments they refer to.

That is a concrete, falsifiable promise. So I looked at what actually reaches the transcript before `γ` is drawn:

```rust
fn append_to_transcript<T: Transcript>(&self, transcript: &mut T) {
    transcript.append(&LabelWithCount(b"rlc_claims", self.0.len() as u64));
    for claim in self.0 {
        claim.evaluation.value.append_to_transcript(transcript);
    }
}
```

Only the claimed values. Not the commitments, not the opening point.

The second hint came from a sibling: the Akita backend in the same repository builds its batch statement with the point once, then the commitment and the value for every claim. When two implementations of the same interface disagree about what gets bound, one of them is usually wrong.

## Building the forgery

With `γ` depending only on the values, a prover can fix the values, compute `γ` by replaying the transcript, and only then pick a polynomial to commit to. Two claims at the same point `z` are enough:

* `p1` is an honest polynomial, but claim 1 says its value is `p1(z) + 1`. That is a lie.
* Claim 2 says `p2(z) = 0`. Before committing to `p2`, compute `γ` from the two values, then choose `p2` to be the constant `γ⁻¹`. Claim 2 is also a lie, since `p2(z) = γ⁻¹`.

Now the combined polynomial is `p1 + γ·p2 = p1 + 1`, which evaluates to `p1(z) + 1` at `z`. The combined claim is `v₁ + γ·v₂ = p1(z) + 1 + 0`. They match, so the opening proof of the combined polynomial is completely honest, and the verifier accepts.

```rust
let v1 = p1.evaluate(&point) + fr(1);   // a lie
let v2 = fr(0);                          // also a lie

// Replay the values-only absorb to learn γ before committing to p2.
let mut replay = Blake2bTranscript::new(b"dory-batch-commitment-binding");
replay.append(&LabelWithCount(b"rlc_claims", 2));
v1.append_to_transcript(&mut replay);
v2.append_to_transcript(&mut replay);
let gamma: Fr = replay.challenge_scalar_powers(2)[1];
let p2 = Polynomial::new(vec![gamma.inverse().unwrap(); 1 << num_vars]);
```

Run through the real `prove_batch` and `verify_batch`, the test that expects a rejection failed on `main`:

```
test dory_homomorphic_batch_binds_commitments_to_the_batching_challenge ... FAILED
panicked: batch verifier accepted two false claims because the batching
         challenge was not bound to the commitments
```

I also checked with a single-claim `DoryScheme::verify` that the first commitment really opens to the true value, so the first claim was genuinely false and not an artifact of the setup.

## Scoping it honestly

Before calling this a soundness bug, I had to ask whether a real Jolt proof could be forged this way. It cannot. In the full zkVM, every commitment is absorbed into the transcript before the first sumcheck stage, and the stage 8 opening point is derived from the challenges of stages 1 to 7. So by the time `γ` is drawn, the transcript already depends on the commitments, just not through the batch itself. Jolt even had a test for exactly this (`dory_clear_final_opening_batch_requires_commitment_binding`).

So the accurate description was: the local contract of a public API was broken, and its safety depended on an unwritten precondition that every caller had to meet. I wrote the report that way, with the scope section near the top, and opened an issue rather than a PR because a full fix changed the proof transcript in several places. I prepared and tested that fix anyway, including the end to end prove and verify paths and the regenerated fuzz fixtures, and later opened it as [#1936](https://github.com/a16z/jolt/pull/1936).

## The resolution

The maintainers chose the other fix: keep the transcript, correct the contract. Their reasoning was that the crate already follows a "caller binds" convention. The single-claim `open` and `verify` never absorb the commitment either, and the zero-knowledge batch path draws `γ` with no absorbs at all. Binding commitments in the clear batch alone would have made the API inconsistent. [#1937](https://github.com/a16z/jolt/pull/1937) rewrote the `BatchOpeningScheme` and `Statement` docs to state the caller's obligation, closed the issue, and left proofs, fixtures and the Fiat-Shamir inventory untouched.

That is a good outcome. The precondition that every integrator of `jolt-openings` has to meet is now written down where the next person will look for it, instead of being something the code silently relied on.

## What I take from it

* **Docs are a specification.** A sentence like "the commitments are bound to the transcript" is a test waiting to be written. The fastest way to find real bugs is to find a promise and check whether the code keeps it.
* **Siblings disagree for a reason.** When one backend binds the point and commitments and the other does not, at least one of them is wrong, or the difference deserves a comment.
* **Build the attack through the public API.** A two-line forgery against `prove_batch` and `verify_batch` is far more convincing than an argument about transcripts, and it becomes the regression test.
* **State the scope before anyone asks.** Saying up front that this was not exploitable in the zkVM, and why, kept the conversation about the right fix instead of about severity.
