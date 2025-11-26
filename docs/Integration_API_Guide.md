# Basic requirements

## 1. Spendbase account onboarding

You should have an active account. You can create an account via the link app.spendbase.co > Money > Get started. Then, you will pass onboarding, and we will review your data and onboard your company.

## 2. Certificate Sign Request Guide

### Generate a private key

Run this in a secure location on your machine (do not share the private key):

    openssl genrsa -out client.key 2048

- This creates a 2048-bit RSA key called client.key
- Keep this file secret — it stays with you.

### Create a CSR

Now use the private key to generate the CSR:

    openssl req -new -key client.key -out client.csr

You’ll be prompted for some fields:

- Country Name (C): two-letter code (e.g., US, DE)
- State or Province (ST): full name (e.g., California)
- Locality Name (L): city (e.g., San Francisco)
- Organization Name (O): your company/organization
- Organizational Unit (OU): optional (e.g. IT Department)
- Common Name (CN): unique identifier for the client — usually your company name or a system name. It will be used to identify your certificate during authentication.
- Email Address: optional
- Challenge password: leave it empty 

### Send us the CSR

- Provide us with the file client.csr on spendbase.api@spendbase.co
- Do not send client.key — keep it private.
  
We’ll sign your CSR with our CA and return a certificate (client.crt), which you’ll use along with client.key to connect securely.

## 3. External Token

An external token is required for API authentication. We will provide the external token in reply to the email.

# Integration

## Auth

All requests should include the External-Token: token header for auth and Content-Type: application/json if a body is provided. 

## TLS

Certificates “client.cert” and “client.key” are required for TLS. The browser certificate manager could be used for cert import.

Another way is using curl with params for API requests:
    
    curl --cert client.crt --key client.key

# Documentation

BASE_URL=https://cards-integration-api.dev.spendbase.co (dev)

## 1. Accounts

### Get a list of all accounts

GET /accounts/accounts/:currency

- Currency (EUR, USD…) as path param. 


    curl --cert client.crt --key client.key \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/accounts/accounts/EUR

The response includes sub-accounts and the master-account in the requested currency. Account’s data should contain ID, name, balance info, etc.

### Create account

POST accounts/account

Body:

- accountName: name of the new account (string)
- currencyISONum (978 for EUR): currency special numeric code (string)


    curl --cert client.crt --key client.key -X POST \
    -H "External-Token: $EXTERNAL_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"accountName": "ExampleName","currencyISONum": "978"}' $BASE_URL/cards-adapter/v1/public/accounts/account

Responds with the new account name and ID if creation succeeded.

### Transfer money between accounts

POST /accounts/accounts-transfer

Body:

- amount: transfer amount (float)
- accountId: transfer to ledger id (string)
- sourceAccountId: transfer from ledger id (string) 
- currencyISONum: currency special numeric code (string)
- Ledger IDs could be received from accounts or card info routes (get all accounts, get all cards).


    curl --cert client.crt --key client.key -X POST \
    -H "External-Token: $EXTERNAL_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"accountId": "$ledgerID","amount": 0.1,"currencyISONum": "978","sourceAccountId": "$sourceLedgerID"}' $BASE_URL/cards-adapter/v1/public/accounts/accounts-transfer

Responds with a success message if the money was transferred.

## 2. Cards

Most card routes require a card ID as a path parameter. ID could be received from the list of cards or the card info routes.

### Create card

POST /cards/card

Body:

- accountId:  card will be created on the account with the given ID (string)
- email: cardholder email (string)
- cardName: custom name for card (string)


    curl --cert client.crt --key client.key -X POST \
    -H "External-Token: $EXTERNAL_TOKEN" \
    -H "Content-Type: application/json" \
    -d '{"accountId": "$id","cardName": "AdapterTestCardEUR2","email": "example@spendbase.co"}' $BASE_URL/cards-adapter/v1/public/cards/card

Responds with a success message if the card was created.

### Get card

GET /cards/card/:id

    curl --cert client.crt --key client.key \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/cards/card/$id

Returns card object with name, id, currency, account info, etc.

### Get all cards

GET /cards/cards

    curl --cert client.crt --key client.key \ 
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/cards/cards

Returns a list of card objects with IDs, names, accounts, currencies info, etc.

### Get card details

GET /cards/card-details/:id

    curl --cert client.crt --key client.key \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/cards/card-details/$id

Returns card financial details.

### Lock card

PUT /cards/lock-virtual-card/:id
No required body.

    curl --cert client.crt --key client.key -X PUT \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/cards/lock-virtual-card/$id

Responds with a success message if the card’s status was changed.

### Unlock card

PUT /cards/unlock-virtual-card/:id
No required body.

    curl --cert client.crt --key client.key -X PUT \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/cards/unlock-virtual-card/$id

Responds with a success message if the card’s status was changed.

### Terminate card

PUT /cards/terminate-virtual-card/:id
No required body.

    curl --cert client.crt --key client.key -X PUT \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/cards/terminate-virtual-card/$id

### Set limit

POST /cards/set-virtual-card-limit/:id

Body:

- type: “DAILY”, “MONTHLY”, “WEEKLY”, “UNLIMITED”, “FIXED” (string)
- amount: limit value to set (int)


    curl --cert client.crt --key client.key -X PUT \
    -H "External-Token: $EXTERNAL_TOKEN" \ 
    -H "Content-Type: application/json" \
    -d '{"amount": 1000,"type": "DAILY"}' $BASE_URL/cards-adapter/v1/public/cards/set-virtual-card-limit/$id

Responds with a success message if the card limit update was requested successfully.

## 3. Transactions

### Get all cards tx list

GET /transactions/card-transactions

Query params:

- from: start timestamp (unix int)
- to: end timestamp (unix int)
- accountName: get for specific account (string)


    curl --cert client.crt --key client.key \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/transactions/card-transactions?to=1754042551&from=1753864397&accountName=TEST account

Returns card transactions list with tx IDs, card IDs, names, amounts, etc.

### Get tx details by id

GET /transactions/transaction/:id


    curl --cert client.crt --key client.key \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/transactions/card-transaction-details/$id

### Get tx status

GET /transactions/transaction-status/:id

    curl --cert client.crt --key client.key \
    -H "External-Token: $EXTERNAL_TOKEN" $BASE_URL/cards-adapter/v1/public/transactions/transaction-status/$id

### Add note to tx

POST /transactions/note/:id

Body:

- note: attachment message for transaction (string)


    curl --cert client.crt --key client.key \
    -X POST -H "Content-Type: application/json" \
    -H "External-Token: $EXTERNAL_TOKEN"\
    -d '{"note": "test"}'
    $BASE_URL/cards-adapter/v1/public/transaction
    s/note/$id
