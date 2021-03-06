# Bitcoin Core with ``addrindex`` Patch

Bitcoin Core is used by Counterparty to interact with the Bitcoin blockchain.  However, vanilla Bitcoin Core is insufficient---instead, a version patched to enable an 'address index' is required.


## Download

Depending on your OS and other preferences, download one of the binaries or source code from the link below. Optionally you can verify the file's checksum and author's PGP signature. These binaries are built deterministically.

[https://github.com/btcdrak/bitcoin/releases](https://github.com/btcdrak/bitcoin/releases) 


## Installation

### Windows Installer

Unlike the Linux binaries, the Windows packages are installers. Uninstall any older version and download and install a 32-bit or 64-bit version. A 32-bit version consumes slightly less memory and is recommended for regular desktop use.

* 32-bit: `C:\Program Files (x86)\Bitcoin`
* 64-bit: `C:\Program Files\Bitcoin`

`bitcoind.exe` can be found in the `daemon` subdirectory. This path can be manually added to the system PATH variable.

### Linux Binaries

The Linux binaries are precompiled executables + dependencies, and they're deployed by decompressing them into the desired location. Once that is done, they can be executed directly like so. 

        ./bitcoin-0.10.0/bin/bitcoind --help

You may want to do one or more of the following optional steps:
* Move the binaries around.
* Edit your `PATH` variable to allow you to execute the binaries (`bitcoin-cli`, `bitcoind`, `bitcoin-qt`) without specifying their path.


## Configuration

Your Bitcoin Core configuration file should match this:

    rpcuser=bitcoinrpc
    rpcpassword=<password>
    server=1
    daemon=1
    rpcthreads=1000
    rpctimeout=300
    txindex=1
    addrindex=1

* Choose a **secure password**.
* By default, on Windows, this file is located at `%APPDATA%\Bitcoin\bitcoin.conf`.
* By default, on Linux, this file is located at `~/.bitcoin/bitcoin.conf`, and the permissions of the file should be set with `chmod 600 ~/.bitcoin/bitcoin.conf`.

## Usage

To run with the standard GUI interface, start Bitcoin Core (`./bitcoin-qt` on Linux), and to run the daemon, execute `bitcoind`. A full list of options can be obtained like this:
* Bitcoin Core: go to `Help > Command-line options` or execute `./bitcoin-qt --help`
* bitcoind: run `./bitcoind --help` (`bitcoind.exe --help` on Windows)

Use `bitcoin-cli` to interact with Bitcoin Core.

### Usage on Testnet 

For testnet use, add `testnet=1` to a separate copy of the above configuration file or run bitcoind with `-testnet` from a script or the console. Examples:
* `./bitcoind -testnet` - start bitcoind on testnet using the default configuration file (blockchain data would be stored in the default data path under `testnet3` subdirectory)
* `bitcoind.exe -testnet -datadir=E:\testing\testnet-blockchain` - use the default configuration file, run on testnet, but place blockchain data in the specified data directory
* `bitcoin-qt.exe -conf=E:\testing\testnet.conf` - start Bitcoin Core GUI and service using a custom configuration file (which presumably cointains `testnet=1` and other custom options that justify the creation of a testnet-specific configuration file)

To interact with a testnet instance of Bitcoin Core, use `bitcoin-cli` with the same `testnet` or `conf` options that were used to start it.

## Reindex

If this is not the first time you are running Bitcoin Core (with `addrindex` or `txindex`) on this computer,
you'll need to launch ``bitcoind`` (once only) as follows:

    bitcoind -reindex

This will have `bitcoind` complete a one-time reindexing of the local blockchain. Windows users can do the same, or simply run Bitcoin-Core (`Start > Programs > Bitcoin Core > Bitcoin Core`) which will prompt them to reindex their blockchain if necessary.

Even on a fast machine, reindexing of the entire mainnet blockchain takes hours.
