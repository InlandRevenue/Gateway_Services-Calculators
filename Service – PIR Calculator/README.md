![IRD logo](../Images/IRlogo.gif)
![Software Dev](../Images/SoftwareDev.png)

# Prescribed Investor Rate Service 
---
#### Release version 1.0
The PIR Calculator API described in this build pack document provides a mechanism for external partners to retrieve the suggested PIR (Prescribed Investor Rate) for a customer.

>**NOTE:** The PIR calculator API Service is only available to Digital Service Providers who use X.509 Digital Certificate used for Mutual TLS on port 4046 and requires OAuth2 or JWT token.

## Key Documentation
---
- YAML file:
	- View and download the [PIR Calculator YAML](PIR%20Calculator.yaml)

- Build Pack 
	- [Download the build pack](Build%20pack%20-%20Prescribed%20Investor%20Rate%20Service.pdf) to view data definitions of each operation and response status code definitions
	
- Message Samples
	* [View message samples for requests and responses](#-message-samples)

## Environment Information
- [Mock Environment Information - Emulated Services, MindMap and Test data](#-mock-environment-information)

- [Test Environment Information - Test Scenarios Report Template and URL Endpoints](#-test-environment-information)

- [Production Environment Information - URL Endpoint](#-prod-environment-information)

## Supporting services
---
- Service: Identity and Access - view: [How to integrate, OAuth requests and responses message samples and build pack](https://github.com/InlandRevenue/Gateway_Services-Access/tree/master/Identity%20and%20Access)

## Message Samples
---
* Sample JSON payload messages
	* Requests
	    * [Request with mandatory attributes only](sample%20messages/request_with_mandatory_attributes.json)
	    * [Request with optional attributes: DOB, FormattedName and FormattedAddress](sample%20messages/request_with_optional_attributes_1.json)
	    * [Request with optional attributes: DOB, Name and Address](sample%20messages/request_with_optional_attributes_2.json)
	    
	* Positive responses
	    * [Positive response with Suggested PIR rate](sample%20messages/response_with_SuggestedPirRate.json)
	    * [Positive response with PIR not found](sample%20messages/response_with_PirRateNotFound.json)
	  
	* Negative response - http 400
	    * [EV1000 - No incoming POST Content found](sample%20messages/response_EV1000_NoIncomingPost.json)
	    * [EV1100 - Invalid input parameters. Field: FormattedName](sample%20messages/response_EV1100_InvalidInputParametersFormattedName.json)
	    * [EV1100 - Invalid input parameters. Field: IRD](sample%20messages/response_EV1100_InvalidInputParametersIRD.json)

	* Negative response - http 401
	    * [EV1023 - No customer associated to TLS cert](sample%20messages/response_EV1023_NoCustomerAssoicatedToTLSCert.json)
	    * [EV1024 - Authentication error](sample%20messages/response_EV1024_AuthenticationError.json)
	    * [EV1025 - Missing token](sample%20messages/response_EV1025_MissingToken.json)
	    * [EV1026 - Authentication error, Issued date or expiry date is invalid](sample%20messages/response_EV1026_AuthenticationErrorInvalidDate.json)
	    * [EV1027 - Certificate or credential could not be found](sample%20messages/response_EV1027_CredentialNotFound.json)
	    * [EV1028 - JWT signature validation failed](sample%20messages/response_EV1028_JWTSignatureFailed.json)
	    * [EV1029 - JWT algorithm not configured](sample%20messages/response_EV1029_JWTAlgorithmNotConfigured.json)
	    * [EV1041 - Logon associated to OAuth token does not have access to customer](sample%20messages/response_EV1041_LogonOauthTokenDoesNotHaveAccess.json)
	    * [EV1042 - Unable to validate API consumer to JWT provided](sample%20messages/response_EV1042_UnableToValidateConsumerToJWT.json)
 	    * [EV1043 - Unable to validate JWT to IRD number provided](sample%20messages/response_EV1043_UnableToValidateJWTToMember.json)
   
## Mock Environment Information
---
### Mock Emulated service URL
| End point|  URL|
|--|--|
| Mock | https://mock-pir.ird.digitalpartner.services/secure/gateway/calculators/pir |

### Mock scenarios MindMap

- [View larger image](images/PIR%20Calculator%20API%20Mock%20Service%20Mindmap.png)
![Mock Scenarios](images/PIR%20Calculator%20API%20Mock%20Service%20Mindmap.png)

### Test data

   - The following test data can be tested in our Mock Services environment when submitting requests to the service operations
   - This table shows which scenarios (as per their numbers in the mindmap) require specific data to trigger the expected responses.

      Scenario ID | Data | Http status | Response 
    	--- | --- | --- | ---
    	PIR-EM-001 | IRD# = 139276507 | 200 | Suggested PIR 28%
    	PIR-EM-002 | IRD# = 139276272 | 200 | Suggested PIR 17.5%
    	PIR-EM-003 | IRD# = 139276310 | 200 | Suggested PIR 10.5%
    	PIR-EM-004 | any IRD# not used in any other scenario. Example: 123987654 | 200 | PIR not found
    	PIR-EM-005.01 | IRD# = 139276329 with header authorization containing word bearer | 401 | EV1041 - Logon does not have access
    	PIR-EM-005.02 | IRD# = 139276329 with header authorization not containing word bearer or without header authorization | 401 | EV1043 - Unable to validate JWT token
    	PIR-EM-006 | IRD# not provided in request payload | 400 | EV1100 - Invalid Input parameters. Field IRD
    	PIR-EM-007 | Any IRD#. Example: 123456789 with Address and Country = "AB" | 400 | EV1100 - Invalid Input parameters. Field Country
    	PIR-EM-008 | Any IRD# using invalid json format. Example: IRD# = 1234567890123 | 400 | EV1100 - Invalid Input parameters... (end of error message can vary according to json format)
    	PIR-EM-009 | Any IRD# using a wrong endpoint. Example: https://mock-pir.ird.digitalpartner.services/secure/gateway/calculators/wrong | 404 | Not found
    	PIR-EM-010 | Using method GET | 405 | Method Not Allowed

>**NOTE:** The emulated service is not managing authentication. Access delegation/restriction is not emulated and any user has access to the test data.

## Test environment information
---
### Test environment URL
| End point|  URL|
|--|--|
| Testing | https://test5.services.ird.govt.nz:4046/gateway/calculators/pir |    
| Pre-Production | https://test4.services.ird.govt.nz:4046/gateway/calculators/pir | 

>**NOTE:** These endpoints are subject to change due to environment updates in the future. 
### Test scenarios MindMap

- [View larger image](images/PIR%20Calculator%20API%20Onboarding%20scenarios%20Mindmap.png)
![Test Scenarios](images/PIR%20Calculator%20API%20Onboarding%20scenarios%20Mindmap.png)

### Test scenarios report template

- [Download Test Scenarios report template](PIR%20Calculator%20Service%20-%20Test%20Report%20Template.docx)

## Prod environment information
---
### Prod environment URL
| End point|  URL|
|--|--|
| Production | https://services.ird.govt.nz:4046/gateway/calculators/pir |

