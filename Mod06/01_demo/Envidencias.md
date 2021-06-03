### Demo: Register an app with the Microsoft Identity platform

In this demo you'll learn how to perform the following actions:

- Register an application with the Microsoft identity platform

#### Prerequisites

This demo is performed in the Azure Portal.

##### Login to the Azure Portal

1. Login to the portal: [https://portal.azure.com](https://portal.azure.com/)

#### Register a new application

1. Search for and select **Azure Active Directory**. On the **Active Directory** page, select **App registrations** and then select **New registration**.

2. When the **Register an application** page appears, enter your application's registration information:

   | Field                       | Value                                                        |
   | --------------------------- | ------------------------------------------------------------ |
   | **Name**                    | az204appregdemo                                              |
   | **Supported account types** | Select **Accounts in this organizational directory**         |
   | **Redirect URI (optional)** | Select **Public client/native (mobile & desktop)** and enter http://localhost in the box to the right. |

   Below are more details on the **Supported account types**.

   | Account type                                                 | Scope                                                        |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | **Accounts in this organizational directory only**           | This option maps to Azure AD only single-tenant (only people in your Azure AD directory). |
   | **Accounts in any organizational directory**                 | This option maps to an Azure AD only multi-tenant. Select this option if you would like to target anyone from any Azure AD directory. |
   | **Accounts in any organizational directory and personal Microsoft accounts** | This option maps to Azure AD multi-tenant and personal Microsoft accounts. |

3. Select **Register**.

   ![img](https://www.skillpipe.com/api/2.1/content/urn:uuid:88438492-2a00-5769-bee1-e4c9ebc889fb@2020-12-12T08:30:18Z/OEBPS/Images/906136-363583.png)

Azure AD assigns a unique application (client) ID to your app, and you're taken to your application's **Overview** page.

✔️ **Note:** Leave the app registration in place, we'll be using it in future demos in this module.