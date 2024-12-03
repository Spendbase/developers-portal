# Documentation for TPP Integration

For detailed API specifications and request-response formats, refer to the [consent application documentation](https://doc.tribepayments.com/docs/3d7a5400-e1a8-11ec-8b58-2e8a68cda573/45a08efe-e1a8-11ec-84d5-de0474a3e416/357b60e4-2f62-11ef-9537-beb6bcae26cf#introduction).

[Spendbase Bank Swagger](https://api-dev.spendbase.co/cards-router/swagger/index.html#/)

The consent application documentation link provides a comprehensive overview of the available endpoints and parameters for interacting with the consent application APIs.

As part of the integration flow, TPPs will first call the consent application API, which in turn communicates with Spendbase’s Open Banking API to retrieve or initiate data. TPPs do not need direct access to Spendbase’s APIs. Instead, they interact with the consent application API, which serves as an intermediary to securely access Spendbase's data on behalf of the user.

This documentation will guide TPPs in making the appropriate API calls to the consent application, ensuring secure and compliant integration with Spendbase Bank's Open Banking infrastructure.
