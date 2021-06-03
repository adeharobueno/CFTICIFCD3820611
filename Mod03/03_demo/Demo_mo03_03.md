```powershell
#Connect to Azure 
Connect-AzAccount

# Prompts for a region for the resources to be created
# and generates a random name for the resources using
# "az204" as the prefix for easy identification in the portal

$myLocation = Read-Host -Prompt "Enter the region (i.e. westus): "
$myResourceGroup = "az204-blobdemo-rg"
$myStorageAcct = "az204blobdemo" + $(get-random -minimum 10000 -maximum 100000)

# Create the resource group
New-AzResourceGroup -Name $myResourceGroup -Location $myLocation

# Create the storage account
New-AzStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAcct `
    -Location $myLocation -SkuName "Standard_LRS"


Write-Host  "`nNote the following resource group, and storage account names, you will use them in the code examples below.
    Resource group: $myResourceGroup
    Storage account: $myStorageAcct"
```

Copy the Connection string storage account:

```
Access Key:
DefaultEndpointsProtocol=https;AccountName=az204blobdemo37545;AccountKey=/TFA7U2msE/b76RTjnkajQPkr+kfAKAJpxTNyicsAb2ePpUMVciqb4hheNTOYQGUQ5xWOzTD0RkSbJxMALxI4w==;EndpointSuffix=core.windows.net
```

```
cd .\Mod03\

dotnet new console -n az204-blobdemo

cd .\az204-blobdemo\

dotnet build

dotnet add package Microsoft.Azure.Storage.Blob

code .
```

copy the next code in program.cs

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;
using static System.Console;

namespace az204_blobdemo
{
    class Program
    {
        public static void Main()
        {
            WriteLine("Azure Blob Storage Demo\n");

            // Run the examples asynchronously, wait for the results before proceeding
            ProcessAsync().GetAwaiter().GetResult();

            WriteLine("Press enter to exit the sample application.");
            ReadLine();

        }

        private static async Task ProcessAsync()
        {
            // Copy the connection string from the portal in the variable below.
            string storageConnectionString = "PLACE CONNECTION STRING HERE";

            // Check whether the connection string can be parsed.
            CloudStorageAccount storageAccount;
            if (CloudStorageAccount.TryParse(storageConnectionString, out storageAccount))
            {
                // Run the example code if the connection string is valid.
                WriteLine("Valid connection string.\r\n");

                // EXAMPLE CODE STARTS BELOW HERE



                //EXAMPLE CODE ENDS ABOVE HERE
            }
            else
            {
                // Otherwise, let the user know that they need to define the environment variable.
                WriteLine("A valid connection string has not been defined in the storageConnectionString variable.");
                WriteLine("Press enter to exit the application.");
                ReadLine();
            }
        }
    }
}
```

Set the storageConnectionString variable to the value you copied from the portal.

Build and run the application to verify your connection string is valid by using the dotnet build and dotnet run commands in your console window.

#### Building the full app

For each of the following sections below you'll find a brief description of the action being taken as well as the code snippet you'll add to the project. Each new snippet is appended to the one before it, and we'll build and run the console app at the end.

For each example below:

1. Copy the code and append it to the previous snippet in the example code section of the *Program.cs* file.

##### Create a container

To create the container, first create an instance of the **CloudBlobClient** object, which points to Blob storage in your storage account. Next, create an instance of the **CloudBlobContainer** object, then create the container. The code below calls the CreateAsync method to create the container. A GUID value is appended to the container name to ensure that it is unique. In a production environment, it's often preferable to use the **CreateIfNotExistsAsync** method to create a container only if it does not already exist. 

**Note: the first line of the next code is missing in skillpipe**

```csharp
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();
// Create a container called 'quickstartblobs' and
// append a GUID value to it to make the name unique.
CloudBlobContainer cloudBlobContainer =
    cloudBlobClient.GetContainerReference("demoblobs" +
        Guid.NewGuid().ToString());
await cloudBlobContainer.CreateAsync();

WriteLine("A container has been created, note the " +
    "'Public access level' in the portal.");
WriteLine("Press 'Enter' to continue.");
ReadLine();
```

##### Upload blobs to a container

The following code snippet gets a reference to a CloudBlockBlob object by calling the GetBlockBlobReference method on the container created in the previous section. It then uploads the selected local file to the blob by calling the UploadFromFileAsync method. This method creates the blob if it doesn't already exist, and overwrites it if it does.

```csharp
// Create a file in your local MyDocuments folder to upload to a blob.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "BlobDemo_" + Guid.NewGuid().ToString() + ".txt";
string sourceFile = Path.Combine(localPath, localFileName);
// Write text to the file.
File.WriteAllText(sourceFile, "Hello, World!");

WriteLine("\r\nTemp file = {0}", sourceFile);
WriteLine("Uploading to Blob storage as blob '{0}'", localFileName);

// Get a reference to the blob address, then upload the file to the blob.
// Use the value of localFileName for the blob name.
CloudBlockBlob cloudBlockBlob = cloudBlobContainer.GetBlockBlobReference(localFileName);
await cloudBlockBlob.UploadFromFileAsync(sourceFile);

WriteLine("\r\nVerify the creation of the blob and upload in the portal.");
WriteLine("Press 'Enter' to continue.");
ReadLine();
```



##### List the blobs in a container

List the blobs in the container by using the ListBlobsSegmentedAsync method. In this case, only one blob has been added to the container, so the listing operation returns just that one blob. The code below shows the best practice of using a continuation token to ensure you've retrieved the full list in one call (by default, more than 5000).

```csharp
// List the blobs in the container.
WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    // Get the value of the continuation token returned by the listing call.
    blobContinuationToken = results.ContinuationToken;
    foreach (IListBlobItem item in results.Results)
    {
        WriteLine(item.Uri);
    }
} while (blobContinuationToken != null); // Loop while the continuation token is not null.

WriteLine("\r\nCompare the list in the console to the portal.");
WriteLine("Press 'Enter' to continue.");
ReadLine();
```

##### Download blobs

Download the blob created previously to your local file system by using the DownloadToFileAsync method. The example code adds a suffix of “_DOWNLOADED” to the blob name so that you can see both files in local file system.

```csharp
// Download the blob to a local file, using the reference created earlier.
// Append the string "_DOWNLOADED" before the .txt extension so that you
// can see both files in MyDocuments.
string destinationFile = sourceFile.Replace(".txt", "_DOWNLOADED.txt");
WriteLine("Downloading blob to {0}", destinationFile);
await cloudBlockBlob.DownloadToFileAsync(destinationFile, FileMode.Create);

WriteLine("\r\nLocate the local file to verify it was downloaded.");
WriteLine("Press 'Enter' to continue.");
ReadLine();
```



##### Delete a container

The following code cleans up the resources the app created by deleting the entire container using CloudBlobContainer.DeleteAsync. You can also delete the local files if you like.



```csharp
// Clean up the resources created by the app
WriteLine("Press the 'Enter' key to delete the example files " +
    "and example container.");
ReadLine();
// Clean up resources. This includes the container and the two temp files.
WriteLine("Deleting the container");
if (cloudBlobContainer != null)
{
    await cloudBlobContainer.DeleteIfExistsAsync();
}
WriteLine("Deleting the source, and downloaded files\r\n");
File.Delete(sourceFile);
File.Delete(destinationFile);
```



#### Run the code

Now that the app is complete it's time to build and run it. Ensure you are in your application directory and run the following commands:



```powershell
dotnet build
dotnet run
```



There are many prompts in the app to allow you to take the time to see what's happening in the portal after each step.

##### Clean up other resources

The app deleted the resources it created. If you no longer need the resource group or storage account created earlier be sure to delete those through the portal.

```powershell
Get-AzResourceGroup -Name $myResourceGroup | Remove-AzResourceGroup -Force -AsJob
```

