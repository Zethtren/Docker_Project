# Docker App for scraping coinbase pro

This app uses an open-source library I found called copra which can be viewed here https://github.com/tpodlaski/copra

This project can be cloned via 

Then you simply go into the directory you cloned it into and run docker-compose up -d

This will spin up 4 containers. 
- 1 MS SQL instance
- 3 scrapers (BTC-USD, ETH-BTC, ETH-USD)

These scrapers will collect live ticker data and deposit it into your SQL server instance. 

The tables will be named (BTC-USD, ETH-BTC, ETH-USD) respectively and they will be stored in a newly created database called ticks. 

All portions of the instance are designed to reboot automatically if a failure occurs but, for data assurance I recommend running it in two separate locations.

