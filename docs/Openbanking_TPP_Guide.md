# TPP Guide

## 1. TPP Authorization Guide
![/images/1.png](/images/1.png)
The workflow of the authorization:

Preconditions: TPP should register client application in the consent application.

1. TPP sends the authorization request with the Spendbase BIC code to the consent application.
2. The consent application utilizes the BIC code to provide the relevant link to it.
3. The user is then redirected to the Spendbase authorization link, which was identified in the previous step.
4. The user should go through the authorization in the Spendbase.
5. Was the authorization successful?
   - If "no":
     * Spendbase cancels the flow, redirects the user to TPP, and sends the cancellation information. TPP displays cancellation information for PSU.
6. Have the PSU-approved scopes?
   - If "no":
     * Spendbase cancels the flow, redirects the user to TPP, and sends the cancellation information. TPP displays cancellation information for PSU
   - If "yes":
     * Spendbase redirects users to the TPP site with authorization data in the URL query.
7. TPP sends a request with authorization data from the URL query.
8. The consent application responds with authorization tokens and other information.

## 2. TPP Get Data Guide
![/images/2.png](/images/2.png)
The workflow of the get info:

1. TPP sends the request with the required object to the consent application.
2. The consent application checks if the scopes are approved.
   - If "no":
     * The consent application sends the error message to TPP that authorization did not succeed.
   - If "yes":
     * TPP receives the data it requested.

## 3. Production Link Request for TPP

Please note that the link to the Spendbase production environment will be provided separately upon request. This is to ensure controlled access and additional security measures in the live environment.