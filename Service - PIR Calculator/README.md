![IRD logo](../Images/IRlogo.gif)
![Software Dev](../Images/SoftwareDev.png)

# PIR Calculator Service
---
#### Release version 1.0

The Prescribed Investor Rate (PIR) calculator service is a REST API that provides a suggested prescribed investor rate for an individual customer.

#### How the PIR calculator service works
Income from PIE (portfolio investment entity) investments is taxed at a rate called the prescribed investor rate or PIR.

Because PIRs are chosen when a customer opens an account but the tax is squared up at a later date, the PIR may be out of date. The PIR calculator service can assist in this scenario by providing a suggested prescribed investor rate for an individual customer.

The customer in consultation with their PIE provider is responsible for using the correct prescribed investor rate.

>**NOTE:** The PIR calculator API Service is only available to Digital Service Providers who use X.509 Digital Certificate used for Mutual TLS on port 4046 and requires OAuth2 or JWT token.

## Key documentation
---
- YAML file:
	- View and download the [PIR Calculator YAML](PIR%20Calculator%2020230308.yaml)

- Build pack 
	- [Download the PIR Calculator Service build pack](Build%20pack%20-%20Prescribed%20Investor%20Rate%20Service.pdf) to view data definitions of each operation and response status code definitions
	
- Message samples
	* [View message samples for requests and responses](#MessageSamples)

## Environment information
- [Mock environment information - emulated services, mind map, test scenarios report template and test data](#MockEnvironmentInformation)
- [Production environment information - URL endpoint](#ProdEnvironmentInformation)

## Supporting services
---
- Service: Identity and Access - view: [How to integrate, OAuth requests and responses message samples and build pack](https://github.com/InlandRevenue/Gateway_Services-Access/tree/master/Identity%20and%20Access)

<a name="MessageSamples"></a>
## Message samples
---
* Sample JSON payload messages
	* Requests
	    * [Request with mandatory attributes only](sample%20messages/request_with_mandatory_attributes.json)
	    * [Request with optional attributes: PieIRD, DOB, FormattedName, FormattedAddress](sample%20messages/request_with_optional_attributes_1.json)
	    * [Request with optional attributes: PieIRD, DOB, Name, Address](sample%20messages/request_with_optional_attributes_2.json)
		* [Request with optional attributes: PieIRD, FilingPeriod](sample%20messages/request_with_optional_attributes_2.json)
	    
	* Positive responses
	    * [Positive response with Suggested PIR rate](sample%20messages/response_with_SuggestedPirRate.json)
	    * [Positive response with PIR not found](sample%20messages/response_with_PirRateNotFound.json)
	  
	* Negative response - http 400
	    * [EV1000 - No incoming POST Content found](sample%20messages/response_EV1000_NoIncomingPost.json)
	    * [EV1100 - Invalid input parameters. Field: FormattedName](sample%20messages/response_EV1100_InvalidInputParametersFormattedName.json)
	    * [EV1100 - Invalid input parameters. Field: IRD](sample%20messages/response_EV1100_InvalidInputParametersIRD.json)
		* [EV1100 - Invalid input parameters. Field: FilingPeriod](sample%20messages/response_EV1100_InvalidInputParametersFilingPeriod.json)
		* [PER100 - FilingPeriod provided is not a valid period on customer](sample%20messages/response_PER100_InvalidFilingPeriod.json)
		* [PIR101 - PieIRD must be provided. (sample%20messages/response_PIR101_PieIRDIsRequired.json)
		
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
   
   
<a name="MockEnvironmentInformation"></a>   
## Mock environment information
---
### Mock emulated service URLs
| End point|  URL|
|--|--|
| Landing Page| https://pir.test.services.ird.govt.nz |
| Mock PIR operation | https://pir.test.services.ird.govt.nz/gateway/calculators/pir |

### Mock scenarios mind map

- [View larger image](images/PIR%20Calculator%20API%20Mock%20Service%20Mindmap.png)
![Mock Scenarios](images/PIR%20Calculator%20API%20Mock%20Service%20Mindmap.png)

- [Onboarding Scenarios](images/PIR%20Calculator%20API%20Onboarding%20scenarios%20Mindmap.png)
![Onboarding Scenarios](images/PIR%20Calculator%20API%20Onboarding%20scenarios%20Mindmap.png)

### Test data

- The following test data can be tested in our Mock Services environment when submitting requests to the service operations
- This table shows which scenarios (as per their numbers in the mindmap) require specific data to trigger the expected responses.

|     Scenario   ID    	|     Data                                                                                                                  	|     Http   status    	|     Response                                                                                           	|
|----------------------	|---------------------------------------------------------------------------------------------------------------------------	|----------------------	|--------------------------------------------------------------------------------------------------------	|
|     PIR-EM200 1      	|     IRD# = 139276507                                                                                                      	|     200              	|     Suggested PIR 28%                                                                                  	|
|     PIR-EM200 2      	|     IRD# = 139276310                                                                                                      	|     200              	|     Suggested PIR 10.5%                                                                                	|
|     PIR-EM200 3      	|     IRD# = 139276272, PieIRD = Any 9 digit number                                                                            	|     200              	|     Suggested PIR 17.5%                                                                                	|
|     PIR-EM200 4      	|     IRD# = 139439976                                                                                                      	|     200              	|     Suggested PIR 10.5%                                                                                	|
|     PIR-EM200 5      	|     any IRD# not used in any other   scenario. Example: 123987654                                                         	|     200              	|     PIR not found                                                                                      	|
|     PIR-EM401 1      	|     IRD# = 139276329 with header   authorization containing word bearer                                                   	|     401              	|     EV1041 - Logon does not have   access                                                              	|
|     PIR-EM401 2      	|     IRD# = 139276329 with header   authorization not containing word bearer or without header authorization               	|     401              	|     EV1043 - Unable to validate   JWT token                                                            	|
|     PIR-EM400 1      	|     IRD# not provided in request   payload                                                                                	|     400              	|     EV1100 - Invalid Input   parameters. Field IRD                                                     	|
|     PIR-EM400 2      	|     Any IRD#. Example: 123456789   with Address and Country = "AB"                                                        	|     400              	|     EV1100 - Invalid Input   parameters. Field Country                                                 	|
|     PIR-EM400 3      	|     Any IRD# using invalid json   format. Example: IRD# = 1234567890123                                                   	|     400              	|     EV1100 - Invalid Input   parameters... (end of error message can vary according to json format)    	|
|     PIR-EM400 4      	|     Add FilingPeriod field not in   the yyyy-MM-dd format                                                                 	|     400              	|     EV1100 - Invalid Input   parameters. FilingPeriod                                                  	|
|     PIR-EM400 5      	|     Add FilingPeriod with any year   8years or more from now    e.g. "2015-03-31"                                         	|     400              	|     PER100 - FilingPeriod provided   is not a valid period on customer.                                	|
|     PIR-EM400 6      	|     Any FilingPeriod with any   month != 03 or day != 31    e.g. "2023-01-31" or "2023-03-01"                             	|     400              	|     PER100 - FilingPeriod provided   is not a valid period on customer.                                	|
|     PIR-EM400 7      	|     Any FilingPeriod with any   future filing date     e.g. "2099-03-31"                                                  	|     400              	|     PER100 - FilingPeriod provided   is not a valid period on customer.                                	|
|     PIR-EM400 8      	|     IRD# = 139332754 or IRD# = 139276272                                                                                     	|     400              	|     PIR101 - PieIRD must be provided.                                                                	|
|     PIR-EM404 1      	|     Any IRD# using a wrong endpoint.   Example: https://mock-pir.ird.digitalpartner.services/gateway/calculators/wrong    	|     404              	|     Not found                                                                                          	|
|     PIR-EM405 1      	|     Using method GET                                                                                                      	|     405              	|     Method Not Allowed                                                                                 	|

>**NOTE:** The emulated service is not managing authentication. Access delegation/restriction is not emulated and any user has access to the test data.

### Test scenarios report template

- [Download Test Scenarios report template](PIR%20Calculator%20Service%20-%20Test%20Report%20Template.docx)

<a name="ProdEnvironmentInformation"></a>  
## Prod environment information
---
### Prod environment URL
| End point|  URL|
|--|--|
| Production | https://services.ird.govt.nz:4046/gateway/calculators/pir |
