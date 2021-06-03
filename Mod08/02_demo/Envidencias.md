### Demo: Import an API by using the Azure Portal

In this demo you'll learn how to perform the following actions:

- Import an API
- Test the new API

✔️ **Note:** This demo relies on the successful deployment of the APIM instance created in the “*Create an APIM instance by using Azure CLI*” demo.

#### Prerequisites

This demo is performed in the Azure Portal.

##### Login to the Azure Portal

1. Login to the portal: [https://portal.azure.com](https://portal.azure.com/)

#### Go to your API Management instance

1. In the Azure portal, search for and select **API Management services**.

   

   ![](images\906120-363567.png)

   

2. On the **API Management** screen, select the API Management instance you created in the *Create an APIM instance by using Azure CLI* demo.

#### Import and publish a backend API

This section shows how to import and publish an OpenAPI specification backend API.

1. Select **APIs** from under **API MANAGEMENT**.

2. Select **OpenAPI specification** from the list and click **Full** in the pop-up.

   ![img](https://www.skillpipe.com/api/2.1/content/urn:uuid:88438492-2a00-5769-bee1-e4c9ebc889fb@2020-12-12T08:30:18Z/OEBPS/Images/906125-363572.png)

   You can set the API values during creation or later by going to the **Settings** tab. The red star next to a field indicates that the field is required. Use the values from the table below to fill out the form.

   | Setting                   | Value                                               | Description                                                  |
   | ------------------------- | --------------------------------------------------- | ------------------------------------------------------------ |
   | **OpenAPI Specification** | https://conferenceapi.azurewebsites.net?format=json | References the service implementing the API. API management forwards requests to this address. |
   | **Display name**          | *Demo Conference API*                               | If you press tab after entering the service URL, APIM will fill out this field based on what is in the json. This name is displayed in the Developer portal. |
   | **Name**                  | *demo-conference-api*                               | Provides a unique name for the API. If you press tab after entering the service URL, APIM will fill out this field based on what is in the json. |
   | **Description**           | Provide an optional description of the API.         | If you press tab after entering the service URL, APIM will fill out this field based on what is in the json. |
   | **URL scheme**            | *HTTPS*                                             | Determines which protocols can be used to access the API.    |
   | **API URL suffix**        | *conference*                                        | The suffix is appended to the base URL for the API management service. API Management distinguishes APIs by their suffix and therefore the suffix must be unique for every API for a given publisher. |
   | **Tags**                  | Leave blank                                         | Tags for organizing APIs. Tags can be used for searching, grouping, or filtering. |
   | **Products**              | Leave Blank                                         | Products are associations of one or more APIs. You can include a number of APIs into a Product and offer them to developers through the developer portal. |
   | **Version this API?**     | Leave unchecked                                     | For more information about versioning, see [Publish multiple versions of your API](https://docs.microsoft.com/en-us/azure/api-management/api-management-get-started-publish-versions) |

3. Select **Create**.

#### Test the API

Operations can be called directly from the Azure portal, which provides a convenient way to view and test the operations of an API.

1. Select the API you created in the previous step (from the **APIs** tab).

2. Press the **Test** tab.

3. Click on **GetSpeakers**. The page displays fields for query parameters, in this case none, and headers. One of the headers is Ocp-Apim-Subscription-Key, for the subscription key of the product that is associated with this API. The key is filled in automatically.

4. Press **Send**.

   Backend responds with **200 OK** and some data.

✔️ **Note:** We will continue to leverage this API in demos throughout the remainder of this module.