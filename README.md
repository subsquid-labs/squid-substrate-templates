# Templates for Substrate squids

This repository is a collection of simple [Subsquid](https://www.subsquid.io)-based indexers ("squids") for scraping blockchain data from [Substrate](https://substrate.io)-based chains such as Polkadot, Kusama, Moonbeam, Astar, Shibuya etc. They are useful as starting points for new projects indexing the following:

* **substrate** - for all projects involving Substrate
* **frontier-evm** - chains with Frontier EVM support (Moonbeam, Moonriver, Shiden, Astar)
* **ink** - Ink!-based WASM contracts, supported e.g. by Astar and Shibuya networks
* **acala** - EVM+ contracts deployed to Acala or Karura network

To begin using these templates, install the `sqd` excutable:
```bash
npm install -g @subsquid/cli@latest
```
Next, fetch the chosen template to a new project folder using `sqd init`:
```bash
sqd init <new-project-name> -t <substrate|frontier-evm|ink|acala>
```
You now have a copy of the chosen squid template sitting at the `<new-project-name>` folder.

## Starting the squid

All Substrate squid templates are complete simple squids. They can run locally immediately upon being downloaded, provided that `sqd` and Docker are available.

The "processor" process is present in all squids. It downloads pre-filtered blockchain data from [Subsquid Archives](https://docs.subsquid.io/archives/), applies any necessary transformations and saves the result in an [application-appropriate storage](https://docs.subsquid.io/basics/store/). All template squids store their data to a Postgresql database and provide a way to access it via a GraphQL API.

To start the processor, run
```bash
cd <new-project-name>
npm ci
sqd build
sqd up # starts a Postgres database in a Docker container
sqd process
```
Processor should now be running in foreground, printing messages like
```
01:02:37 INFO  sqd:processor 234354 / 16519081, rate: 17547 blocks/sec, mapping: 2420 blocks/sec, 2945 items/sec, ingest: 794 blocks/sec, eta: 16m
```
The extracted data will begin accumulating in the database available at `localhost:23798` (consult the template's `.env` file for login and password). If you want to access it via GraphQL, run
```bash
sqd serve
```
in a separate terminal in the `<new-project-name>` folder. Graphical query builder will be available at [http://localhost:4350/graphql](http://localhost:4350/graphql).

## Further reading

* [Substrate quickstart guide at Subsquid docs](https://docs.subsquid.io/quickstart/quickstart-substrate/): a more detailed version of this README that gets updated before this file does.
* [Squid development flow](https://docs.subsquid.io/basics/squid-development/): a guide to reshaping a template into your own squid.
* (possibly still under construction)[Substrate indexing](https://docs.subsquid.io/substrate-indexing/): the comprehensive documentation section on indexing Substrate.
* [Tutorials](https://docs.subsquid.io/tutorials/) and [examples](https://docs.subsquid.io/examples).
