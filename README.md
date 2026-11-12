### ecamli

Onchain analyst and content strategist. Most of my code time goes into finding and fixing bugs in Web3 protocol repositories, each one with a failing test first.

**Merged fixes**

* **x402-foundation/x402** (TypeScript, Python)
  [#4225](https://github.com/x402-foundation/x402/pull/4225) skip fully charged channels in batch-settlement refunds ·
  [#4227](https://github.com/x402-foundation/x402/pull/4227) strip the internal Settlement-Overrides header when the handler fails
* **ProjectOpenSea/seaport-js** (TypeScript)
  [#1057](https://github.com/ProjectOpenSea/seaport-js/pull/1057) pass fulfillOrder extraData to the zone instead of dropping it (landed in [#1058](https://github.com/ProjectOpenSea/seaport-js/pull/1058)) ·
  [#1063](https://github.com/ProjectOpenSea/seaport-js/pull/1063) drop expired orders from a batch fulfill instead of failing it
* **helius-labs/helius-sdk** (TypeScript)
  [#389](https://github.com/helius-labs/helius-sdk/pull/389) make ws.close() end the standard subscriptions
* **helius-labs/helius-rust-sdk** (Rust)
  [#247](https://github.com/helius-labs/helius-rust-sdk/pull/247) simulate and preflight smart transactions at the blockhash's commitment ·
  [#248](https://github.com/helius-labs/helius-rust-sdk/pull/248) treat an initialized stake account as fully withdrawable ·
  [#249](https://github.com/helius-labs/helius-rust-sdk/pull/249) decode validity proofs and V2 tree accounts as the indexer sends them
* **magicblock-labs/magicblock-validator** (Rust)
  [#1903](https://github.com/magicblock-labs/magicblock-validator/pull/1903) resolve addresses in simulateTransaction inner instructions
* **relayprotocol/relay-kit** (TypeScript)
  [#1143](https://github.com/relayprotocol/relay-kit/pull/1143) keep integer zeros in formatNumber ·
  [#1144](https://github.com/relayprotocol/relay-kit/pull/1144) stop signature step polling on failure or refund ·
  [#1145](https://github.com/relayprotocol/relay-kit/pull/1145) fail when a Tron broadcast is rejected ·
  [#1175](https://github.com/relayprotocol/relay-kit/pull/1175) mark the signature step item as failed when the check fails
* **nansen-ai/nansen-cli** (JavaScript)
  [#818](https://github.com/nansen-ai/nansen-cli/pull/818) keep the leading zero bytes of the to address in wallet send ·
  [#822](https://github.com/nansen-ai/nansen-cli/pull/822) rank Privy x402 payment options and skip methods it cannot sign
* **a16z/jolt** (Rust)
  [#1874](https://github.com/a16z/jolt/pull/1874) make the stack canary check match the linker layout ·
  [#1877](https://github.com/a16z/jolt/pull/1877) resolve ELF symbol names via the symtab's sh_link ·
  [#1902](https://github.com/a16z/jolt/pull/1902) reject MISC-MEM encodings that are not FENCE
* **binius-zk/binius64** (Rust)
  [#2482](https://github.com/binius-zk/binius64/pull/2482) make BinarySubspace::get work when the dimension reaches usize::BITS ·
  [#2483](https://github.com/binius-zk/binius64/pull/2483) reject an RS256 signature that is not below the modulus
* **logos-blockchain/logos-blockchain** (Rust)
  [#3781](https://github.com/logos-blockchain/logos-blockchain/pull/3781) reject channel configs with an unreachable threshold
* **FhenixProtocol/cofhesdk** (TypeScript)
  [#400](https://github.com/FhenixProtocol/cofhesdk/pull/400) ·
  [#401](https://github.com/FhenixProtocol/cofhesdk/pull/401) ·
  [#402](https://github.com/FhenixProtocol/cofhesdk/pull/402) ·
  [#403](https://github.com/FhenixProtocol/cofhesdk/pull/403) ·
  [#464](https://github.com/FhenixProtocol/cofhesdk/pull/464) ·
  [#482](https://github.com/FhenixProtocol/cofhesdk/pull/482) mocks and SDK checks brought in line with the on-chain TaskManager
* **wevm/viem** (TypeScript)
  [#5239](https://github.com/wevm/viem/pull/5239) substitute ERC-1155 token ID as hex in getEnsAvatar metadata URIs ·
  [#5244](https://github.com/wevm/viem/pull/5244) guard consumed nonce 0 in the nonceManager stale-source check ·
  [#5367](https://github.com/wevm/viem/pull/5367) accept an EIP-7702 authorization yParity above 1 in parseTransaction (landed in [#5371](https://github.com/wevm/viem/pull/5371))
* **wevm/ox** (TypeScript)
  [#574](https://github.com/wevm/ox/pull/574) keep the EIP-7702 initCode marker in UserOperation.toPacked (landed in [#593](https://github.com/wevm/ox/pull/593))
* **ethereum/execution-specs** (Python)
  [#3760](https://github.com/ethereum/execution-specs/pull/3760) keep base fee change gas within the parent gas limit
* **flop-labs/technocore-chat** (Python)
  [#892](https://github.com/flop-labs/technocore-chat/pull/892) send if_absent as a JSON boolean from the WebMCP write_note tool

**Reported and fixed upstream**

* [ProjectOpenSea/opensea-sdk#2033](https://github.com/ProjectOpenSea/opensea-sdk/issues/2033) batchApproveAssets approved Multicall3 instead of the user when 2 or more approvals were needed
* [ProjectOpenSea/opensea-sdk#2035](https://github.com/ProjectOpenSea/opensea-sdk/issues/2035) bulkTransfer reverted on every call because its TransferHelper ABI did not match Seaport's
* [ProjectOpenSea/opensea-sdk#2038](https://github.com/ProjectOpenSea/opensea-sdk/issues/2038) the typed camelize no longer mapped template literal keys that the runtime still converted
* [genlayerlabs/genvm-executor](https://github.com/genlayerlabs/genvm-executor/issues?q=is%3Aissue+author%3Aerkancamli) six storage, calldata, EVM encoding and schema bugs, all fixed in the v0.4 line
* [base/base#5182](https://github.com/base/base/issues/5182) baseup swallowed errors raised inside `$(...)`, so a failed install exited with no message
* [tempoxyz/pympp#370](https://github.com/tempoxyz/pympp/issues/370) client re-encoded the challenge `opaque` instead of echoing it, so issuers that bind the raw string rejected the credential
* [getoptimum/optimum-gateway#158](https://github.com/getoptimum/optimum-gateway/issues/158) CL pubsub capped at 1 MiB, so larger blocks were dropped
* [getoptimum/optimum-gateway#186](https://github.com/getoptimum/optimum-gateway/issues/186) attestations from mump2p dropped once the CL subscribed to next-fork topics
* [a16z/jolt#1864](https://github.com/a16z/jolt/issues/1864) HomomorphicBatch drew its RLC challenge without binding the commitments
* [magicblock-labs/magicblock-engine#166](https://github.com/magicblock-labs/magicblock-engine/issues/166) transient account kept a stale prefix after resolving to a larger image
* [flop-labs/yellowpaper](https://github.com/flop-labs/yellowpaper/issues?q=is%3Aissue+author%3Aerkancamli) spec errata and ambiguities, several corrected in the paper

**Review findings that landed**

* [wevm/viem#5436](https://github.com/wevm/viem/pull/5436): ENSIP-11 EVM coin types still lost the leading zeros of an address; the fix was extended to them in [#5442](https://github.com/wevm/viem/pull/5442)
* [wevm/ox#502](https://github.com/wevm/ox/pull/502): an indexed bytes32 argument skipped the length check and short values were padded; fixed before it landed in [#588](https://github.com/wevm/ox/pull/588)

**Writing**

* [Two false claims, one accepted batch](https://github.com/erkancamli/erkancamli/blob/main/writeups/jolt-batch-opening-binding.md): a Fiat-Shamir contract gap in Jolt's batch openings, how I built the forgery through the public API, and why the maintainers fixed the contract instead of the transcript

**Things I built**

* [Gossip Siege](https://github.com/erkancamli/gossip-siege): fan made tower defense game for the Optimum community, where coded shields only break under independent shards. [gossipsiege.xyz](https://gossipsiege.xyz)
* [Reactor](https://github.com/erkancamli/reactor): twelve questions, one block a day; a fan made Rialo knowledge game with every answer linked to its source. [reactors.wtf](https://reactors.wtf)
* [Clause](https://github.com/erkancamli/clause): parametric cover that settles itself, on Rialo
* [technocore-pulse](https://github.com/erkancamli/technocore-pulse): a measuring agent for technocore.chat that publishes signed reports from one DID

[X @ekinoks_26](https://x.com/ekinoks_26)
