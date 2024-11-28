# Spendbase Open Banking Integration Guide

### Document Version Control

| Version | Date | Summary Change |
|---------|------|----------------|
| 1.0 | 34/10/24 | The initial draft |
| 1.1 | 30/10/24 | Revision and correction of CPO |
| 1.2 | 08/11/24 | Contact details added |

## 1. Introduction

This document is intended to provide a comprehensive overview of the integration process for Spendbase within the Open Banking framework. It outlines the steps necessary for authorization and data exchange between Spendbase and third-party providers (TPPs), aiming to ensure secure and efficient interactions. Open Banking allows financial institutions to securely share customer-permitted data with authorized third parties, enhancing transparency and expanding customer access to financial services. This document also serves as a guide for ensuring a seamless user experience, covering the authorization process, data security measures, and integration best practices to enable TPPs to meet user expectations and regulatory compliance.

## 2. Purpose of the Document

The purpose of this document is to serve as a technical guide for integrating TPPs with Spendbase, detailing the required workflows and protocols to establish secure access and data sharing. This guide will be useful for developers and system administrators responsible for implementing and maintaining Spendbase's Open Banking integrations.

In addition to technical workflows, this document provides guidance on designing user-friendly experiences for individuals and businesses using TPP applications or websites. It includes best practices for creating intuitive consent flows, ensuring transparency in data usage, and handling errors to enhance trust and satisfaction for end users.

By following this guide, TPPs can not only meet the technical requirements of integration but also deliver a seamless and secure experience for their users who wish to connect their Spendbase Bank accounts to TPP platforms using Open Banking.

## 3. Definition and Role of TPP

A Third-Party Provider (TPP) is an authorized entity that connects with financial institutions, like Spendbase, to access customer-permitted banking data or initiate transactions on behalf of the customer. For this integration, TPPs must undergo rigorous vetting and certification processes to comply with Open Banking security and authorization standards. It is the responsibility of the TPP to ensure compliance and maintain the required authorizations and certifications.

Users are the clients of TPP applications or websites who wish to connect their Spendbase Bank accounts to the TPP platform using Open Banking. These users can be individuals or businesses seeking to leverage TPP services, such as financial management, payment initiation, or consolidated account information.

## 4. Documentation for TPP Integration

For detailed API specifications and request-response formats, refer to the consent application documentation [link here] and Spendbase Bank Swagger [link here]. The consent application documentation link provides a comprehensive overview of the available endpoints and parameters for interacting with the consent application APIs.

As part of the integration flow, TPPs will first call the consent application API, which in turn communicates with Spendbase's Open Banking API to retrieve or initiate data. TPPs do not need direct access to Spendbase's APIs. Instead, they interact with the consent application API, which serves as an intermediary to securely access Spendbase's data on behalf of the user.

This documentation will guide TPPs in making the appropriate API calls to the consent application, ensuring secure and compliant integration with Spendbase Bank's Open Banking infrastructure.

## 5. TPP Authorization Guide
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

## 6. TPP Get Data Guide
![/images/2.png](/images/2.png)
The workflow of the get info:

1. TPP sends the request with the required object to the consent application.
2. The consent application checks if the scopes are approved.
   - If "no":
     * The consent application sends the error message to TPP that authorization did not succeed.
   - If "yes":
     * TPP receives the data it requested.

## 7. Production Link Request for TPP

Please note that the link to the Spendbase production environment will be provided separately upon request. This is to ensure controlled access and additional security measures in the live environment.

## 8. Open Banking User Guides

These guides help users to set up open banking connections between TPP and Spendbase Bank and manage these open banking connections.

### Authorization Guide

1. To add a Spendbase Bank account to the TPP application/website, the user must first select Spendbase Bank and specify the consents they wish to grant access to within the TPP application/website.
2. The TPP application/website will navigate you to the Spendbase Open Banking Login screen with the consent information.
![/images/3.png](/images/3.png)
3. Enter your credentials and click "Login".
4. Once the system navigates the user to the "Two-Factor Authentication" screen.
5. Enter the 2FA code and click "Verify".
![/images/4.png](/images/4.png)
6. After successful verification, you will be directed to the "Account Selection" screen. Select the account that you want to grant access to and click "Confirm".
![/images/5.png](/images/5.png)
7. After successful authorization in Spendbase Bank, the user will be redirected to the TPP application/website via a callback URL with an "authorization_code," which the TPP can exchange for an access token and use it for the Get Data Flow (described in Section 5).

### Consent Management Guide

1. User sign-in to Spendbase Bank website. Users can go to the Open Banking Consent Management Screen, where they can see all their Open Banking connections.

2. Revoking User Consent
   - From Spendbase Bank website
     ![/images/6.png](/images/6.png)
     * To revoke consent, the user must go to the Open Banking Consent Management Screen, choose the active Open Banking connection, and click on it.
     * Then click the "Disable Connection" button.
     ![/images/7.png](/images/7.png)
     * After these actions, the Open Banking connection will be revoked.
   - From TPP application/website
     * Each TPP may have a unique method for handling user consent revocation. This could range from in-app settings to customer support channels.

## 9. Contact Information

For any questions or support with implementation, please contact us via support@spendbase.co

Please include "Open Banking" in the subject line of your requests.
