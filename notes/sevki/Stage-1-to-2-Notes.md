# Chain Data Analysis

In order to collect data from chains there are various ways like transaction API, or [open source clients](./resources/Stage-1-to-2.md) (see `data-clients` section). However, there is no focused API on exporting accounts code, or related data rabout smart contract execution.

In order to have ability to export data from chains, I have started to create a local development environment. Right now, I am able to debug Geth client with GDB tool on an Ubuntu laptop. 

It is also possible to print logs, however the client code needs to be build every time you update your logs.

I am working on a Light Geth client, I will inspect the data structure further and track the data related smart contracts.

Please see the [geth-clef](./resources/Stage-1-to-2.md) section for creating such a dev environment.

Also see [my notes](./resources/gdb-commands.md) on getting on the gdb tool up-and-running