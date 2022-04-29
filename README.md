# Quai Manager

Official Golang implementation of the Quai Manager.

## Building the source

For prerequisites and detailed build instructions please read the [Installation Instructions](https://docs.quai.network/develop/mining).

Building `quai-manager` requires both a Go (version 1.14 or later) and a C compiler. You can install
them using your favourite package manager. Once the dependencies are installed, run

Build via Makefile

```shell
make quai-manager
```

Build via GoLang directly

```shell
go build -o ./build/bin/manager manager/main.go
```

## Run the manager

### Setting the region and zone flags for mining location

With the introduction of the auto-miner enhancement, it is now possible to let the manager automatically find and set itself to the best location. In this mode, the manager will start at the best location. There is an also an option to "optimize," and if set true it will also check periodically whether it is still in the best location and, if not, it will update to the best location. The best location is the chain with the lowest observed difficulty, meaning in auto-miner mode the manager automatically selects the chain likely to bring the best rewards to a miner while also automatically distributing hashrate across the network evenly.

In the file config.yaml you should see something like this:

```
# Config for node URLs
Location: [1,1]
Auto: true
Mine: true
Optimize: true
OptimizeTimer: 10
PrimeURL: "url"
RegionURLs: "urls"
ZoneURLs: "urls"
```

This file is responsible for storing your settings. The settings saved in this file on starting the manager are what will be applied when it runs.

Location: this stores the Region and Zone values for setting the mining location manually. (Will only be used if Optimize is set to false.) Values must correspond to the current Quai Network Ontology. At mainnet launch, the values for Region will be 1-3 and for Zone 1-3. So, for example, to mine on Region 2 Zone 3 you would save the Location value like this:

```
Location: [2,3]
```

Auto: if true, then the miner will automatically find and select the best location on start up. It must be set to false if you want to manually set the location.

Mine: if true, the miner will mine. This value must be set true in order to mine. If it is set false, then the manager will not mine (though it will perform other functions, such as subscribing to the chains so it will stay updated).

Optimize: if true, then periodically the manager will check the difficulty of the chains and, if a chain with lower difficulty is found, it will switch to that location. If set to false, it will only mine at the location set at startup.

OptimizeTimer: this value represents how many minutes between Optimize checks the manager will make. By default the value is set to 10.

PrimeURL: stores the URL for the Prime chain. Should not be changed.

RegionURLs: stores the URLs for the Region chains. Should not be changed.

ZoneURLs: stores the URLs for the Zone chains. Should not be changed.

The below command runs the manager:

Run via Makefile

```
make run
```

The manager can also be run in the background with logs saved to a file. It can be run similarly to make run, with the same auto-miner and optimizer enhancements possible.

To run in the background:

```
make run-background
```


### Set

Run via Go binary

```shell
./build/bin/manager 1 2
```

## Stopping the manager

```shell
make stop
```
