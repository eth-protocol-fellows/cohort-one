# Project Dive-in (Update 1):

## Smart Contracts in Java
#### The EIP: [EIP-4780](https://github.com/jeyakatsa/EIPs/blob/master/EIPS/eip-4780.md)
#### The Work: [Java Smart Contract Abstraction for Ethereum](https://github.com/jeyakatsa/Ethereum-Smart-Contract-Java-Abstraction)
#### The Research & Development process: [Java Smart Contract Abstraction for Ethereum R&D](https://github.com/jeyakatsa/ethereum-smart-contract-java-abstraction/tree/main/R%26D-files) 
_________________________________________________________________________

## New-ERC Token
### The Proposal: [New-ERC Token Propoal](https://ethresear.ch/t/a-new-erc-token-proposal/11540)
#### The EIP: [EIP-XXXX](https://github.com/jeyakatsa/EIPs/blob/master/EIPS/eip-XXXX.md)[number to be determined]
#### The Research & Development process: [New-ERC Token R&D](https://github.com/jeyakatsa/New-ERC-Token/blob/main/R&D.md)
#### The Work: [New-ERC Token Build](https://github.com/jeyakatsa/New-ERC-Token)
_________________________________________________________________________

## Altair - Minimal Light Client Prototype Build 
### [Light-Client Custom Token Creation Proposal](https://ethresear.ch/t/light-client-custom-token-creation-proposal/11433)
#### The Research & Development process: [Step-By-Step Guide](https://hackmd.io/ZFINvY5fRUGrLK-BteZrug?view)
#### The Work: [Prototype Server & Demo](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype)
_________________________________________________________________________ 

#### Daily Updates (October 2021 - December 2021):
- [Saturday 12/4/21]: Started work on ZeroHash function within [Utilities](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/utilities/Utilities.java) class. Eclipse removed as coding tool from [Readme](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/README.md) & [Step-By-Step Guide](https://hackmd.io/ZFINvY5fRUGrLK-BteZrug?view) (as was the primary arbiter of compilation issues for IntelliJ). [Light-Client Server "Module"](https://github.com/jeyakatsa/teku/tree/master/light-client) refactored from Eclipse for IntelliJ and all import issues resolved via [gradle.build](https://github.com/jeyakatsa/teku/blob/master/light-client/build.gradle). IntelliJ compilation issue resolved, solution recorded: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md).
- [Friday 12/3/21]: New trial & error approach being implemented to resolve compilation issues, results recorded: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md).
- [Thursday 12/2/21]: Continuing diagnosis of IntelliJ issue.
- [Wednesday 12/1/21] Working on redirecting Gradle specified location, progress & discovery recorded here: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md).
- [Tuesday 11/30/21] Tried implementing `.gradle` into variable to resolve IntelliJ compilation issue, results recorded: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md).
- [Monday 11/29/21] Currently *Redirecting Environmental Variables* to fix IntelliJ compilation issue, results recorded: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md).
- [Sunday 11/28/21]: Currently *self diagnosing* IntelliJ compilation issue, results recorded: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md). 
- [Saturday 11/27/21] IntelliJ refusing to reveal compilation errors while building, cannot continue build until problem resolved. *Currently in conversation with Teku team* as well as *a self diagnosis* to resolve. 
- [Friday 11/26/21] "Generic Arrays" not supported within Java, "ArrayLike" solution refactored, results recorded: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md), ArrayLike class deleted. Downloaded/Installed new IDE (IntelliJ) to match with Teku team specifics, mentioned [here](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/README.md).
- [Thursday 11/25/21] "ArrayLike" function problem solved, results recorded here: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md). Moving onto refactoring [ArrayLike class](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/server/ArrayLike.java) for Utilities class.
- [Wednesday 11/24/21] All possible classes tested (within SSZ), results recorded here: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md).
- [Tuesday 11/23/21] Search completed for "ArrayLike" function within Ssz packages, results recorded here: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md).
- [Monday 11/22/21] Search for "ArrayLike" function continues within Teku client, results recorded here: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md).
- [Sunday 11/21/21] Continuing search for "ArrayLike" function within Teku client recorded here: [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md). Conversating with the Teku team on Discord to expediate the process.
- [Saturday 11/20/21] Created [R&D Technical Paper](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/Teku-Light-Client-Server-R%26D.md) to help speed up problem solving issues while building the server.
Started work on [Utilities](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/server/Utilities.java).
Completed [LightClientStore](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/client/LightClientStore.java).
Completed [LightClientUpdate](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/client/LightClientUpdate.java). Fully completed [SyncCommittee](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/client/SyncCommittee.java).
- [Friday 11/19/21] Started work on a Step-By-Step Build Process Techincal Post (for future Cohort that would like to follow/build along the process) https://hackmd.io/ZFINvY5fRUGrLK-BteZrug?view.
- [Thursday 11/18/21] All functions found and recorded within Lodestar-Light-Client-Build-Reference (*Condensed into Step-By-Step process (https://hackmd.io/ZFINvY5fRUGrLK-BteZrug?view)*). Added more server reference reviews (https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/blob/main/README.md). In conversation on Discord with the Teku team at Consensys to find a more efficient proposal for the build.
- [Tuesday 11/16/21] Double check completed (function not found), seeking help through Chainsafe #light-client Discord.
- [Monday 11/15/21]: Search for Number64UintType() function completed (function not found), double checking links for function within Lodestar-Light-Client-Build-Reference.
- [Sunday 11/14/21]: Search for Number65UintType() function continues as more links added within Lodestar-Light-Client-Build-Reference.
- [Saturday 11/13/21]: Lodestar-Light-Client-Build-Reference added in order to search referred files for build (*Condensed into Step-By-Step process (https://hackmd.io/ZFINvY5fRUGrLK-BteZrug?view)*). Searching for Number65UintType() within Chainsafe branch.
- [Friday 11/12/21]: Minor notes added within LightClientStore.
- [Thursday 11/11/21]: LightClientSnapshot separated out of Types, LightClientStore created (https://github.com/jeyakatsa/teku/tree/master/light-client/src/main/java/tech/pegasys/teku/lightclient/client). 
- [Wednesday 11/10/21]: LightClientSnapshot completed (https://github.com/jeyakatsa/teku/commit/d4924fb474db31757d152bda50ccea38663b4768).
- [Tuesday 11/9/21]: Server build process created (for Light Client API) (*Condensed into Step-By-Step process (https://hackmd.io/ZFINvY5fRUGrLK-BteZrug?view)*)
- [Monday 11/8/21]: Started work on thinly veiled Rest API.
- [Sunday 11/7/21]: LightClientSnapshot build commenced, SyncCommittee class separated.
- [Saturday 11/6/21]: Gradle build successfully completed, SyncCommittee built, tested and pull requested (https://github.com/ConsenSys/teku/pull/4579).
- [Friday 11/5/21]: Still working to resolve compilation issues: https://github.com/ConsenSys/teku/pull/4532.
- [Thursday 11/4/21]: Compilation issues being resolved _continued_.
- [Wednesday 11/3/21]: Compilation issues being resolved (noted within link below).
- [Tuesday 11/2/21]: Gradle build 99% completed: https://github.com/ConsenSys/teku/pull/4532
- [Monday 11/1/21]: Started building Light Client API infastructure, more Gradle dependency issues being resolved concurrently.
- [Sunday 10/31/21]: Gradle connection/configuration issue resolved!
- [Friday 10/29/21]: Still resolving Gradle connection/configuration issue.
- [Thursday 10/28/21]: Currently debugging Gradle connection/configuration issue.
- [Wednesday 10/27/21]: Resolving Gradle connection/configuration issues (referenced within pull request on Tuesday 10/26/21).
- [Tuesday 10/26/21]: Opened pull request for Light Client logic for Teku (https://github.com/ConsenSys/teku/pull/4532). Continuing light client build.
- [Monday 10/25/21]: Started work on Beacon Block Header and SyncCommittee.
- [Sunday 10/24/21]: Light client store, light client snapshot and Sync committee logic started within types class. 
- [Saturday 10/23/21]: Light client packages and classes created into Teku. [Light-Client R&D aggregator](https://github.com/ethereum-cdap/cohort-one/blob/master/notes/Light-Clients-R&D.md) launched. 
- [Friday 10/22/21]: Updated light client server reference. Built light client skeleton server into Teku client.
- [Thursday 10/21/21]: Wireframe completed. Moving onto building Light Client Demo logic (as well as the server infastructure into Teku).
- [Wednesday 10/20/21]: Updated Wireframe design.
- [Tuesday 10/19/21]: Opened issue about Light Client Demo error (https://github.com/ChainSafe/eth2-light-client-demo/issues/11). Working on wireframe.
- [Monday 10/18/21]: Tested Lodestar Light Client Demo against Epoch, not working. Opening issue with Chainsafe. Updated Wireframe Guideline.
- [Sunday 10/17/21]: Built out skeleton for Demo as well as Icons and Wireframe.
- [Saturday 10/16/21]: Created separate Prototype Demo and Server projects.
- [Friday 10/15/21]: First batch of Classes and Interfaces laid out, moving onto building the logic and checking the batch against the Teku reference.
- [Thursday 10/14/21]: PrototypeBuildingBlocks notes completed (for now), Validator Package classes and interfaces now being built.
- [Wednesday 10/13/21]: Added Classes and Interfaces to Validator package Clients and BeaconNode within PrototypeBuildingBlocks notes.
- [Tuesday 10/12/21]: Added more Source Folders, Packages, etc to Prototype.
- [Monday 10/11/21]: Added Validator Package.
- [Sunday 10/10/21]: Prototype skeleton build recommenced.
- [Saturday 10/9/21]: Prototype skeleton build placed on hold, resolving new dependency importing issue before building can commence.
- [Thursday 10/7/21]: Refactoring prototype skeleton based off Gradle specifics (off Teku).
- [Wednesday 10/6/21]: Teku dependency import issue resolved (mostly) *issue closed*. Skeleton for prototype to be reorganized and built with Eclipse/Gradle diametrics (build reference added to README).
- [Tuesday 10/5/21]: Downloaded/Installed Gradle and Eclipse, moving onto testing Teku dependency imports.
- [Monday 10/4/21]: Downloading/installing Gradle and Eclipse to try and resolve issues. *Spring Tool Suite not condusive to building Beacon Chain Client in Java (due to Teku specifics).*
- [Sunday 10/3/21]: Added node interface to main prototype package, validation packages to source test (source folder) and other base classes. Opened issue ticket with Consensys in regards to Teku reference (https://github.com/ConsenSys/teku/issues/4437).
- [Saturday 10/2/21]: Added specs and rebuilt skeleton based off Teku specifics and dependencies, converting specs into Java (from Python).
- [Friday 10/1/21]: Light client specs integrated into skeleton(as Python), Java conversion next.
- [Thursday 9/30/21]: Refactoring prototype skeleton based off Teku specifics.
- [Wednesday 9/29/21]: Prototype Skeleton Built. Validation issue closed (due to potential irrelevancy to testing out Altair Client prototype (might jump back on this later (dependent on relevancy))). Forked teku repo (https://github.com/jeyakatsa/teku).
- [Monday 9/27/21]: Opened repo to begin building out Minimal Light Client Prototype (https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype). Keystore validation issue *almost* resolved (https://github.com/ChainSafe/lodestar/issues/3249). Still working on resolving Local Testnet connection issue locally on terminal.
- [Sunday 9/26/21]: Opened validation connection issue, currently working on resolving Local Testnet connection issue locally on terminal.
- [Saturday 9/25/21]: Ran multiple installations, validation error still occurring, moving onto opening issue with Chainsafe before moving onto resolving local testnet connection issue.
- [Thursday 9/23/21] Lodestar Beacon sync (with all networks) ran and tested. Sync error passed to Chainsafe core dev to resolve (https://github.com/ChainSafe/lodestar/issues/3220). Currently working on resolving "Keystores not found within network file" issue for Validation (quite possibly through installation type)... running multiple installations to chip away at issue.~~
- [Wednesday 9/22/21] Lodestar Beacon synching error with Chainsafe currently being resolved (synch still processing)..~~
- [Tuesday 9/21/21] Had to restart synch in order to open synch issue with Chainsafe (taking much longer than anticipated (24+ hours) to synch), might need another 24hrs+ since synch restarted.
- [Monday 9/20/21] Connected to Testnet, synching Lodestar Beacon Node with terminal (awaiting sync to complete). Logging errors to open GitHub issue reports through Chainsafe when possible.
- [Sunday 9/19/21] Installed Yarn and Lodestar on Git terminal, moving onto connecting Mainnet.
