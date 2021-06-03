### Demo: Transform your API by using policies

In this demo you'll learn how to perform the following actions:

- Add a policy to an API to strip response headers

By stripping response headers you can hide the information about the technology stack that is running on the backend.

✔️ **Note:** This demo relies on the successful completion of the *Create an APIM instance by using Azure CLI* and *Import an API by using the Azure Portal* demos.

#### Prerequisites

This demo is performed in the Azure Portal.

##### Login to the Azure Portal

1. Login to the portal: [https://portal.azure.com](https://portal.azure.com/)

#### Go to your API Management instance

1. In the Azure portal, search for and select **API Management services**.

   ![img](Images\906120-363567.png)

2. On the **API Management** screen, select the API Management instance you created in the *Create an APIM instance by using Azure CLI* demo.

3. Select **APIs** in the navigation pane.

4. Select the **Demo Conference API**.

#### Test the original response

1. Select the **Test** tab, on the top of the screen.

2. Select the **GetSpeakers** operation.

3. Press the **Send** button, at the bottom of the screen. The response includes the response headers below:

   x-aspnet-version: 4.0.30319 x-powered-by: ASP.NET

#### Define the policy

1. On the top of the screen, select **Design** tab.

2. Select **All operations**.

3. In the **Outbound processing** section, click the **</>** icon.

4. Modify your <outbound> code to look like this:

   <outbound>    <set-header name="X-Powered-By" exists-action="delete" />    <set-header name="X-AspNet-Version" exists-action="delete" />    <base /> </outbound>

5. Select **Save**.

6. Inspect the **Outbound** process section and note there are two set-header policies listed.

#### Test the modified response

1. Select the **Test** tab, on the top of the screen.
2. Select the **GetSpeakers** operation.
3. Press the **Send** button, at the bottom of the screen.

The response no longer includes platform information related to your API.