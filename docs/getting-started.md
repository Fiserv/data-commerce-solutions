<!-- 
type: tab 
titles: Before You Start, Know Our Standard API Structure, Make Your First API Call
-->

# Before You Start
<!-- theme: info -->
> #### Note for Developers
>
>
>
Before you start integration, it is important to register on the Fiserv Developer Studio to test the Account API in the Sandbox environment. You may choose to test APIs using the <a href="?path=docs/getting-started/make-your-first-api-call.md#using-third-party-api-testing-tools" >Third-party API Testing Tools</a> of your choice. However, registration is not required to learn about our APIs and reference documentation.

## Register on Fiserv Developer Studio
This section describes the process to create an account and workspace on Fiserv Developer Studio to obtain credentials for sandbox testing.

### Creating an Account

Perform the following steps to create an account on Fiserv Developer Studio:
1.	From the top-right corner of the screen, click **Create account**
2.	Populate the required fields and click **Next**
3.	Follow the instructions on the screen to set up your account
4.	Sign in to your Fiserv Developer Studio account once it is activated

### Creating a FiservMe Workspace

Workspaces are dedicated spaces for developers to obtain API key, API secret and product related details.

Perform the following steps to create a workspace on Fiserv Developer Studio:

1.	Sign in to your Fiserv Developer Studio account
2.	From the top-right corner of the screen, click **Workspaces**. My Workspace page displays
      <!-- theme: info -->
      > #### Note
      >
      > All previously created workspaces are listed on the **My workspaces** page.

3.	To create a new workspace, click the **Add a new workspace** button or click the **Create a new workspace** card. Create a workspace dialog box displays
4.	Enter workspace name and description

Every workspace has following three sections:

* **Summary**: Displays workspace details and list of activities performed on the workspace
* **Credentials**: Lists all active API keys. From this section, you can view or download the following details of an API key:
    * Product name: _Name of the product, for example, Account Verification_
    * Org ID: _Organization ID is required to send in all API requests under the <a href="?path=docs/getting-started/know-our-standard-api-structure.md#request-body" >Request Header</a>_
    * API key name: _Name of the API key_
    * API key type: _Type of API key, for example, Trial_
    * API key: _Alphanumeric value of the API key. API key is used as username while generating the access token_
    * API secret: _Alphanumeric value of the API secret. API secret is used as password while generating the access token_
    * Host URL: _Host URL path to send API requests_
* **Settings**: Used to modify or delete the workspace

## Generating Access Token

An access token is used to authenticate your API build and allows you to use the Fiserv APIs securely. **API key** and **API secret** values obtained from the Workspace are required to generate an access token.

Use the API mentioned below to generate an access token using Postman.

### URL

``POST https://connect-dev.fiservapis.com/v1/account/token/generate ``


### Headers

|     Header Name      |     Description                                          |     Required      |
|---------------------|----------------------------------------------------------|---------------|
|     ``Authorization`` |    <p>Base64 encoded string representing your username and password values, appended to the text Basic as follows: </p> <p> <code> Basic <Base64 encoded username and password> </code></p> <p> **Important:** In Postman, use the **Authorization** tab to enter Username and Password values and set authentication type to **Basic Auth**. Use your **API key** as username and **API secret** as password. </p>                      |     Required    |

![image](https://github.com/Fiserv/data-commerce-solutions/raw/develop/assets/images/auth_api_request.png)


### Request Body

From the Body tab, select the **x-www-form-urlencoded** radio button and enter the following key-value pair:

``grant_type = client_credentials``

![image](https://user-images.githubusercontent.com/81968767/220961197-8e76ec1f-b291-4dfd-8287-c4a83b4ecf40.png)

### Response
From the response we are only interested in the following fields:
access_token, expires_in and token_type.

|     Field Name      |     Description                                          |     Type      |
|---------------------|----------------------------------------------------------|---------------|
|   ``access_token``    |     Generated access token   value                       |     string    |
|``expires_in``       | <p>Time in milliseconds until the generated token is valid.</p> <p>**Note:** Once generated, the access token is valid for approximately 15 minutes. You can reuse the access token until it expires. </p> | number        |
|    ``token_type``   |     Type of access token                                 |     string    |

**Sample Response**
```
{
    "refresh_token_expires_in": "0",
    "api_product_list": "[df-dcs-accountverification-v1ProductX]",
    "api_product_list_json": [
        "df-dcs-accountverification-v1ProductX"
    ],
    "organization_name": "prj-fisv-n-apigeee4aa22a1",
    "developer.email": "integration-tester@fiserv.com",
    "token_type": "BearerToken",
    "issued_at": "1698050494594",
    "client_id": "GYrnxCQE5xos5f00qawk3tT8hKGub1JVgNBQBN95YANBwE9G",
    "access_token": "FdnVuMYAU0hWcZkcEPQPut5yG2EW",
    "application_name": "e0992cc2-3c3c-487b-bfb8-68d25620d57e",
    "scope": "",
    "expires_in": "3599",
    "refresh_count": "0",
    "status": "approved"
}
```


<kbd>
    <img src="https://github.com/Fiserv/data-commerce-solutions/raw/develop/assets/images/Auth-api.gif" />
</kbd>

<!-- type: tab -->

# Know Our Standard API Structure

This section describes a standard structure of request and response message of Account Verification API.

## Request Message

All API requests must contain the following components:

*	[API Method](#api-method)
* [Request URL](#request-url)
* [Access Token](#access-token)
*	[Request Header](#request-header)
*	[Request Body](#request-body)

For every API request, a response message is obtained that contains a response payload and the status of the API request.

### API Method

For security reasons, all API methods are set to POST or PUT, irrespective of the operation.

### Request URL

Request URL is formed by appending Host URL and API path.

<!-- theme: info -->
> **Request URL = Host URL + API path**


To get Host URL, go to API key section of your Workspace. The API path along with the method (POST or PUT) is listed under the API Explorer section of that API on Fiserv Developer Studio.
Refer to the example if host URL of the product is https://connect-dev.fiservapis.com/v1/account/token/generate/, then request URL will be:

![image](https://github.com/Fiserv/data-commerce-solutions/raw/develop/assets/images/hostPath.png)


### Access Token

An access token is used to authenticate your API build and allows you to use the Fiserv APIs securely. A generated access token is valid for approximately 15 minutes.
To generate an access token, refer to the [Generating Access Token](?path=docs/getting-started.md&branch=develop#generating-access-token) section.



### Request Body

The request body of an API changes based on the type of transaction being processed. Request body contains the detailed information that is required to perform a particular transaction.

**Request Payload**

The following example shows the sample request payload for **Account Verification** API request.

```
{
  "name": {
    "firstName": Name,
    "lastName": Name
  },
  "address": [{
    "addressLine1": "string",
    "city": "string",
    "state": "string",
    "country": "string",
    "zipCode": "string",
    "addressCode": AddressCodeEnum
  }]
}
```


## Response Message

Upon a successful API request, a response payload is received. The response payload contains the status and the returned details of the requested API in key-value pairs. The default response format is JSON.


To view the API documentation of **Get Party List** API in API Explorer, [click here](../api/?type=post&path=/partyservice/parties/parties/secured/list).

<!-- type: tab -->

# Make Your First API Call

This section describes the process to send an API request to the server and receive a response payload. To test the APIs, use the third-party API testing tool.

## Using Third-Party API Testing Tools

You can test our APIs in the Sandbox environment using third-party API testing tools, such as Postman, Apigee, JMeter and others.

<!-- theme: info -->
> #### Tip
>
> We recommend you to refer to the <a href="?path=docs/getting-started/know-our-standard-api-structure.md#know-our-standard-api-structure" title="Click to open">Know Our Standard API Structure</a> section to understand the API structure prior to testing the APIs in any third-party tool.


### Prerequisites
To make an API call, you need:
- An active user account on Fiserv Dev Studio
- An access token


**Creating an account on Dev Studio**


To create an account on Fiserv Developer Studio, refer to the [Register on Fiserv Developer Studio](?path=docs/getting-started/before-you-start.md#register-on-fiserv-developer-studio) section.
After successful registration, you will be able to create a workspace. You can obtain the following credentials from the workspace:

* Product name
* Org ID
* API key name
* API key type
* API key
* API secret
* Host URL

These credentials are important to send in API requests. **API key** and **API secret** values are used to generate access token.


**Generating an Access Token**

An access token is used to authenticate your API build and allows you to use the Fiserv APIs securely. **API key** and **API secret** values obtained from Workspace are required to generate an access token.

To generate an access token, refer to the [Generating Access Token](?path=docs/getting-started/before-you-start.md#generating-access-token) section.


<!-- theme: info -->
> #### Note
>
> Once generated, the access token is valid for approximately 15 minutes. You can reuse the access token until it expires.


### Example

The following example illustrates the process to test an API using Postman application:


Postman is a client that lets you test RESTful APIs. If you are familiar with Postman, refer to the following steps to test Fiserv APIs in the sandbox environment.

<!-- theme: info -->  
> #### Recommendation
>
> Keep the API documentation accessible to refer to the default request-payload for the request message. You may also download the Postman collection and refer to sample API requests from the **Resources** section.

#### Prerequisite to run Postman collection

The attached Postman collection use variables to store and reuse few values such as Hostname, Username and Password. To update the variable values, perform the below steps in Postman.
1.	From the **Collections** tab, select the root folder of Account Verification Postman collection
2.	Select the **Variables** tab
3.	Insert the variable values in the **Current value** column for the following variables:
      * **authToken_UserName**: _API key value_
      * **authToken_Password**: _API secret value_
      * **OrgId**: _Org Id value_

      <!-- theme: info -->
      > #### Note
      >
      > You can obtain the _API key_, _API secret_ and _Org Id_ values from the Credentials tab of My workspaces.


To  test an API using Postman application:

1. Open a web or desktop application of Postman.
2.	Create a new HTTP request
3.	Set the API method to POST or PUT, as mentioned in the API document which you want to test
      <!-- theme: info -->
      > #### Note
      >
      > API method of all Fiserv APIs is either set to POST or PUT for all operations.

4. Insert the request URL
5. Under the **Authentication** tab, select the **Type** value as **Bearer Token** and insert access token in the **Token** box

![image](https://user-images.githubusercontent.com/81968767/220967588-52eec24d-4b13-4d26-ba28-a9ad90943e26.png)

6. Insert the request-payload under the **Body** tab. Make sure that the **raw** radio button is activated and the text format is set to **JSON**

<kbd><img src="https://user-images.githubusercontent.com/81968767/145019152-399b813e-61a6-41c1-9e79-2e3cfd10015f.png" width="70%" /></kbd><br>

 <!-- theme: info -->
> #### Note
>
> Default request-payload can be copied from the API Explorer document and you may modify certain fields as mentioned in the documentation.

7. Modify the field values in JSON code that you want to test
8. Click **Send**. API response is generated in the Response section
<!-- type: tab-end -->