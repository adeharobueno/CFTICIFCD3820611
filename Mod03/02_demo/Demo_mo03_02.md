[Introducción a las cuentas de almacenamiento](https://docs.microsoft.com/es-es/azure/storage/common/storage-account-overview#types-of-storage-accounts)

```
#Connect to Azure 
Connect-AzAccount

$resourcegroup = "newstorage"
$location = "westeurope"
$storageaccount = "bmvnsaabc3"

New-AzStorageAccount -ResourceGroupName newstorage -AccountName $storageaccount -Location $location -SkuName Standard_GRS -Kind BlobStorage -AccessTier Hot
```



```json
{
"rules": [
    {
    "name": "ruleFoo",
    "enabled": true,
    "type": "Lifecycle",
    "definition": {
        "filters": {
        "blobTypes": [ "blockBlob" ],
        "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
        "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
        },
        "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
        }
        }
    }
    }
]
}
```



```json
{
"rules": [
    {
    "name": "Test",
    "enabled": true,
    "type": "Lifecycle",
    "definition": {
               "Actions": {
                 "BaseBlob": {
                   "TierToCool": {
                     "DaysAfterModificationGreaterThan": 30
                   },
                   "TierToArchive": {
                     "DaysAfterModificationGreaterThan": 90
                   },
                   "Delete": {
                     "DaysAfterModificationGreaterThan": 2555
                   }
                 },
                 "Snapshot": {
                   "Delete": {
                     "DaysAfterCreationGreaterThan": 90
                   }
                 }
               },
               "Filters": {
                 "PrefixMatch": [
                   "container1/foo"
                 ],
                 "BlobTypes": [
                   "blockBlob"
                 ]
               }
             }
    }]
}
```



## Para cuentas de almacenamiento tipo BlobStorage

```powershell
#Connect to Azure 
Connect-AzAccount

#Install the latest module if you are running this in a local PowerShell instance
Install-Module -Name Az -Repository PSGallery

#Initialize the following with your resource group and storage account names
$resourcegroup = "newstorage"
$storageaccount = "bmvnsaabc3"

#Create a new action object
$action = Add-AzStorageAccountManagementPolicyAction -BaseBlobAction Delete -daysAfterModificationGreaterThan 2555
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToArchive -daysAfterModificationGreaterThan 90
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToCool -daysAfterModificationGreaterThan 30
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -SnapshotAction Delete -daysAfterCreationGreaterThan 90

# Create a new filter object
# PowerShell automatically sets BlobType as “blockblob” because it is the
# only available option currently
$filter = New-AzStorageAccountManagementPolicyFilter -PrefixMatch "container1/foo"

# Create a new rule object
# PowerShell automatically sets Type as “Lifecycle” because it is the
# only available option currently
$rule1 = New-AzStorageAccountManagementPolicyRule -Name Test -Action $action -Filter $filter

#Set the policy
$policy = Set-AzStorageAccountManagementPolicy -ResourceGroupName $resourcegroup -StorageAccountName $storageaccount -Rule $rule1
```



## Para cuentas de almacenamiento tipo BlockBlobStorage

```
#Connect to Azure 
Connect-AzAccount

$resourcegroup = "newstorage"
$location = "westeurope"
$storageaccount = "bmvnsaabc4"

New-AzStorageAccount -ResourceGroupName newstorage -AccountName $storageaccount -Location $location -SkuName Premium_LRS -Kind BlockBlobStorage -AccessTier Hot
```



```
{
"rules": [
    {
    "name": "Test",
    "enabled": true,
    "type": "Lifecycle",
    "definition": {
        "filters": {
        "blobTypes": [ "blockBlob" ],
        "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
        "baseBlob": {
            "delete": { "daysAfterModificationGreaterThan": 2555 }
        },
        "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
        }
        }
    }
    }
]
}
```





```
#Connect to Azure 
Connect-AzAccount

#Install the latest module if you are running this in a local PowerShell instance
Install-Module -Name Az -Repository PSGallery

#Initialize the following with your resource group and storage account names
$resourcegroup = "newstorage"
$storageaccount = "bmvnsaabc4"

#Create a new action object
$action = Add-AzStorageAccountManagementPolicyAction -BaseBlobAction Delete -daysAfterModificationGreaterThan 2555
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -SnapshotAction Delete -daysAfterCreationGreaterThan 90

# Create a new filter object
# PowerShell automatically sets BlobType as “blockblob” because it is the
# only available option currently
$filter = New-AzStorageAccountManagementPolicyFilter -PrefixMatch "container1/foo"

# Create a new rule object
# PowerShell automatically sets Type as “Lifecycle” because it is the
# only available option currently
$rule1 = New-AzStorageAccountManagementPolicyRule -Name Test2 -Action $action -Filter $filter

#Set the policy
$policy = Set-AzStorageAccountManagementPolicy -ResourceGroupName $resourcegroup -StorageAccountName $storageaccount -Rule $rule1
```



##### Clean up other resources

The app deleted the resources it created. If you no longer need the resource group or storage account created earlier be sure to delete those through the portal.

```
Get-AzResourceGroup -Name $resourcegroup | Remove-AzResourceGroup -Force -AsJob
```



