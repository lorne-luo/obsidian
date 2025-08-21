
## For daily data feed

1. ARM file arrived on time (on the current day)  
    As [CTP-675](https://github.com/selfwealth/product-bigsite-services/pull/743/files) implemented, the database will be updated before 11am, the data feeds step function can query database directly to get the updated description. A flag will be added in DDB Transaction table to indicated whether the description is matched or not.
    
    `SELECT  "TradingSignup"."CashAccountNumber" AS "CustomerAccountId", "AccountTransaction"."ExternalReference" AS "TransactionId",  "AccountTransaction"."Description" FROM "AccountTransaction"     JOIN "Account" ON "AccountTransaction"."AccountId" = "Account"."Id"     JOIN "TradingSignup" ON "TradingSignup"."PortfolioId" = "Account"."PortfolioId" WHERE "TradingSignup"."CashAccountNumber" IN :transaction_ids OR "AccountTransaction"."ExternalReference" IN :custom_account_ids`
    

	This case has been finished in [PR-52](https://github.com/selfwealth/product-b2b-datafeed-cash-transactions/pull/52)

2. ~~ARM file arrives delayed 1-2 days~~  
    ~~Issue: Database will be updated but~~
    

## Historic feed

1. always loads the latest day’s ARM file
2. query unmatched transactions in the past two days’ from DynamoDB
3. if any previously loaded transactions are matched by
    1. account id
    2. transaction date
    3. amount
4. get the description from SAM and dump related transactions in daily feed restult