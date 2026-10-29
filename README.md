### ecamli

Onchain analyst and content strategist. Most of my code time goes into finding and fixing bugs in Web3 protocol repositories, each one with a failing test first.

**Merged fixes**

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
  [#464](https://github.com/FhenixProtocol/cofhesdk/pull/464) mocks and SDK checks brought in line with the on-chain TaskManager
* **wevm/viem** (TypeScript)
  [#5239](https://github.com/wevm/viem/pull/5239) substitute ERC-1155 token ID as hex in getEnsAvatar metadata URIs ·
  [#5244](https://github.com/wevm/viem/pull/5244) guard consumed nonce 0 in the nonceManager stale-source check
* **ethereum/execution-specs** (Python)
  [#3760](https://github.com/ethereum/execution-specs/pull/3760) keep base fee change gas within the parent gas limit
* **flop-labs/technocore-chat** (Python)
  [#892](https://github.com/flop-labs/technocore-chat/pull/892) send if_absent as a JSON boolean from the WebMCP write_note tool

**Reported and fixed upstream**

* [getoptimum/optimum-gateway#158](https://github.com/getoptimum/optimum-gateway/issues/158) CL pubsub capped at 1 MiB, so larger blocks were dropped
* [getoptimum/optimum-gateway#186](https://github.com/getoptimum/optimum-gateway/issues/186) attestations from mump2p dropped once the CL subscribed to next-fork topics
* [a16z/jolt#1864](https://github.com/a16z/jolt/issues/1864) HomomorphicBatch drew its RLC challenge without binding the commitments
* [magicblock-labs/magicblock-engine#166](https://github.com/magicblock-labs/magicblock-engine/issues/166) transient account kept a stale prefix after resolving to a larger image
* [flop-labs/yellowpaper](https://github.com/flop-labs/yellowpaper/issues?q=is%3Aissue+author%3Aerkancamli) spec errata and ambiguities, several corrected in the paper

**Things I built**

* [Gossip Siege](https://github.com/erkancamli/gossip-siege): fan made tower defense game for the Optimum community, where coded shields only break under independent shards. [gossipsiege.xyz](https://gossipsiege.xyz)
* [Reactor](https://github.com/erkancamli/reactor): twelve questions, one block a day; a fan made Rialo knowledge game with every answer linked to its source. [reactors.wtf](https://reactors.wtf)
* [Clause](https://github.com/erkancamli/clause): parametric cover that settles itself, on Rialo
* [technocore-pulse](https://github.com/erkancamli/technocore-pulse): a measuring agent for technocore.chat that publishes signed reports from one DID

[X @ekinoks_26](https://x.com/ekinoks_26)
