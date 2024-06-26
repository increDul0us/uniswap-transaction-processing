# Uniswap Fee Service
- This project provides a backend system for polling and backfilling WETH/USDC uniswap transaction data and calculating the fee in USDT on the time of the transaction.
- It uses Etherscan for fetching the transactions and Binance API for calculating the ETH-USDT rate using the kline API.
- It provides api to fetch fee for any WETH/USDC transaction by providing the transaction hash.
- It also provides an api to backfill for any time range. While handling real time transaction data.

## Requirements
- Docker
- Node Version Manager (nvm)

## Installation
Clone this repository and navigate to the project directory.
```bash
git clone https://github.com/increDul0us/uniswap-transaction-processing
cd uniswap-transaction-processing
```

## Quick start
- Either follow these steps which only uses docker for non-Node.js dependencies

1. Create a local .env file following the fields from the .env.sample and update the keys that you need. If not the basic keys already provided in env.sample should suffice to run. (NB: I temp included etherscan api keys for ease of use)

    ```bash
    cp .env.local .env
    ```

2. We are using Docker for non-Node.js dependencies such as database, rmq. Ensure Docker is ready and run Docker containers.

    ```bash
    docker-compose -p uniswap -f docker-compose.local.yml up -d
    ```

3. Install the defined Node.js version and the project dependencies.

    ```bash
    nvm install && nvm use
    npm i
    ```

4. Run migrations:

    ```bash
    npm run db:migrate
    ```

5. Build and Run the app:

    ```bash
    npm run build
    npm start
    ```

6. Test:

    ```bash
    npm test
    ```


- Or **alternatively** you can quick start with Docker (one-liner) which includes node dependencies:

    ```bash
    cp .env.sample .env
    docker-compose -p uniswap up -d
    docker-compose -p uniswap exec uniswap npm run db:migrate
    docker logs uniswap_uniswap_1 -f
    ```

## Usage
The API is running on port 8001. There is an automatically generated API documentation when server is running. the API can be viewed at [http://localhost:8001/api-docs](http://localhost:8001/api-docs).

### API Endpoints
GET /transaction/backfill: Initiates the backfill process to fetch historical transaction data between specified start and end time in seconds.
```http
GET localhost:8001/transaction/backfill?startTime=1648876800&endTime=1648963200
{
  "status": "success",
  "data": "Backfill process initiated successfully"
}
```
GET /transaction/:hash/fee: Retrieves the fee in USDT for the specified transaction hash.
```http
GET localhost:8001/transaction/0x1c8202b1622cffd8d8bc4dd39b9c6c512d79aebd74acde8443540573675024fd/fee
{
  "status": "success",
  "data": { fee: "24.49" }
}
```
