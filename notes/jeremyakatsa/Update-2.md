# Project Dive-in (Update 2):
## Altair - Minimal Light Client Prototype Build 

### [Light-Client Custom Token Creation Proposal](https://ethresear.ch/t/light-client-custom-token-creation-proposal/11433)

#### The Research & Development process: [Step-By-Step Guide](https://hackmd.io/ZFINvY5fRUGrLK-BteZrug?view)
#### The Work: [Prototype Server & Demo](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype)

_________________________________________________________________________ 

#### Daily Updates (December 2021 - Present):
- [Saturday 12/11/21]: Solving `T[]` constant issue, results recorded: [Light-Client-R&D](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/edit/main/Teku-Light-Client-Server-R&D.md). `assertZeroHashes()` function within [Utilities](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/utilities/Utilities.java) class completed. `rootArray` method within `assertZeroHashes` function within [Utilities](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/utilities/Utilities.java) class problem solved, results recorded: [Light-Client-R&D](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/edit/main/Teku-Light-Client-Server-R&D.md). Refactored [`gradle.build`](https://github.com/jeyakatsa/teku/blob/b2a1e4216b491da1d2fb348dcb424990aab4daa4/light-client/build.gradle) to include `ssz`'s new location within Teku infrastructure.
- [Friday 12/10/21]: `isZeroHash()` function problem resolved within [Utilities](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/utilities/Utilities.java) class, results recorded: [Light-Client-R&D](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/edit/main/Teku-Light-Client-Server-R&D.md).
- [Thursday 12/9/21]: More test cases added to problem solving research, results recorded: [Light-Client-R&D](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/edit/main/Teku-Light-Client-Server-R&D.md).
- [Wednesday 12/8/21]: Trial and error problem solving approach to solve `Bytes32` issue continues, results recorded: [Light-Client-R&D](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/edit/main/Teku-Light-Client-Server-R&D.md).
- [Tuesday 12/7/21]: Added getters and setters to root function in ordinance of trial and error problem solving approach within [Utilities](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/utilities/Utilities.java) class, results recorded: [Light-Client-R&D](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/edit/main/Teku-Light-Client-Server-R&D.md).
- [Monday 12/6/21]: Continuing the process of finding a solution to the `Bytes32` loop error, test cases & findings recorded: [Light-Client-R&D](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/edit/main/Teku-Light-Client-Server-R&D.md).
- [Sunday 12/5/21]: Created [Light-Client Custom Token Creation Proposal](https://ethresear.ch/t/light-client-custom-token-creation-proposal/11433) topic. Currently in the process of refactoring `Bytes32[]` to read `!==` expression in `ZeroHash` function within [Utilities](https://github.com/jeyakatsa/teku/blob/master/light-client/src/main/java/tech/pegasys/teku/lightclient/utilities/Utilities.java) class, results: [Light-Client-R&D](https://github.com/jeyakatsa/Altair----Minimal-Light-Client-Prototype/edit/main/Teku-Light-Client-Server-R&D.md).
