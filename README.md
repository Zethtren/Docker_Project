# Docker App for scraping coinbase pro

This app uses an open-source library I found called copra which can be viewed here https://github.com/tpodlaski/copra

This project can be cloned via:

git clone https://github.com/Zethtren/Docker_Project.git

Then you simply go into the directory you cloned it into and run:

docker-compose up -d

This will spin up 4 containers. 
- 1 MS SQL instance
- 3 scrapers (BTC-USD, ETH-BTC, ETH-USD)

These scrapers will collect live ticker data and deposit it into your SQL server instance. 

The tables will be named (BTC-USD, ETH-BTC, ETH-USD) respectively and they will be stored in a newly created database called ticks. 

All portions of the instance are designed to reboot automatically if a failure occurs but, for data assurance I recommend running it in two separate locations.

If you want to change any part you can freely modify the variables.
As of now the password cannot be modified as I am still working on separating it from the start-up script. I will update this when that change has been made.

If you want to access your sql server from a different port than 1433 simply change the first port number in 

services:
  sql-server:
    image: zethtren/mssql_server:latest
    hostname: sql-server
    container_name: sql-server
    ports:
      - "1433:1433"
      
Example:

services:
  sql-server:
    image: zethtren/mssql_server:latest
    hostname: sql-server
    container_name: sql-server
    ports:
      - "8080:1433"

Now it will be accessed from localhost,8080


If you want to add any additional markets simply check Coinbase pros market list. 

Copy the following code and change it as such

scraper_BTCUSD:
    depends_on: 
      - sql-server
    image: zethtren/scraper:latest
    hostname: scraper_BTCUSD
    container_name: scraper_BTCUSD
    environment: 
      - SA_PASSWORD=asdffdsaasdffdsa1234! #Predefined in DockerFile working on making this adaptable
      - TABLE_NAME=BTC-USD
      - DB_NAME=ticks
    restart: on-failure
    
scraper_LTCUSD:
    depends_on: 
      - sql-server
    image: zethtren/scraper:latest
    hostname: scraper_LTCUSD
    container_name: scraper_LTCUSD
    environment: 
      - SA_PASSWORD=asdffdsaasdffdsa1234! #Predefined in DockerFile working on making this adaptable
      - TABLE_NAME=LTC-USD
      - DB_NAME=ticks
    restart: on-failure
    
Anywhere I had BTC I replaced it with LTC and now this code will create a LTC market you could simply paste this at the end of the original docker file and make sure the spacing aligns. Then you will have 4 markets being scraped.

I have only tested this as high as 3 markets for 24 hours and had absolutely no issues but there is a chance you could run into some. Please let me know if you do!
