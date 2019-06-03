# Cryptostore

[![License](https://img.shields.io/badge/license-XFree86-blue.svg)](LICENSE)
![Python](https://img.shields.io/badge/Python-3.6+-green.svg)
[![PyPi](https://img.shields.io/badge/PyPi-cryptostore-brightgreen.svg)](https://pypi.python.org/pypi/cryptostore)



A storage engine for cryptocurrency market data. You supply the exchanges, data type (trade, book, etc), and trading pairs you're interested in and Cryptostore does the rest!

Stores data to:
* Parquet
* Arctic
* Google Cloud Storage
* Amazon S3

### Requirements

Cryptostore currently requires either Kafka or Redis to be installed. The extra dependencies for your backend of choice must be installed as well (eg `pip install cryptostore[redis]`). Redis requires Redis Streams, which is supported in versions > 5.0.


### Running Cryptostore

Once installed with pip, an executable is placed on the path, so you can simply run `cryptostore` to start the collector. It requires a `config.yaml` file. If its not in the current working directory, you can specify the path to the config with the `--config` option.

An example [config](config.yaml), with documentation inline is provided in the root of the repository. The config file is monitored by cryptostore, so you can change the options in the file and it will apply them without the need to reload the service (this is experimental. If you encounter issues with it, please raise an issue).


### Backfilling Trade Data
Cryptstore can backfill trade data - but be aware not all exchanges support historical trade data, and some only provide a limited amount. The backfill is not designed to be interrupted, so if it is, the backfill start date will need to be changed to the last backfilled trade date.


### Running with other consumers

Cryptostore can operate with other consumers of the exchange data (eg. a trading engine consuming updates).

For Redis
  - Disable the message removal in the Redis settings in `config.yaml`. The other consumer will need to be responsible for
  message removal (if so desired), and it must ensure messages are not removed before cryptostore has had a chance to process them.
  
For Kafka
  - You need only supply a different consumer group id for the other consumers to ensure all consumers receive all messages. Kafka's configuration controls the removal of committed messages in a topic (typically by time or size).


### Planned features
* [ ] Missing data detection and correction (for exchanges that support historical data, typically only trade data)
* [ ] Storing data to InfluxDB
* [ ] Storing data to MongoDB
* [ ] Support for enabling computation and storage of diverse metrics in parallel with data collection (eg. configurable OHLCV)
* [ ] Support for forwarding data to another service/sink (eg. to a trading engine). 
