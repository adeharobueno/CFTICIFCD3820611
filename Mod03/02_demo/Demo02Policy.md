## PowerShell
The following PowerShell script can be used to add a policy to your storage account. The $rgname variable must be initialized with your resource group name. The $accountName variable must be initialized with your storage account name.

```
#Install the latest module if you are running this in a local PowerShell instance
Install-Module -Name Az -Repository PSGallery

#Initialize the following with your resource group and storage account names
$rgname = ""
$accountName = ""

NO supported blockblobstorage --$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToArchive -daysAfterModificationGreaterThan 90

NO supported blockblobstorage ---$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToCool -daysAfterModificationGreaterThan 30

#Create a new action object
$action = Add-AzStorageAccountManagementPolicyAction -BaseBlobAction Delete -daysAfterModificationGreaterThan 2555
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -SnapshotAction Delete -daysAfterCreationGreaterThan 90

# Create a new filter object
# PowerShell automatically sets BlobType as “blockblob” because it is the
# only available option currently
$filter = New-AzStorageAccountManagementPolicyFilter -PrefixMatch ab,cd

# Create a new rule object
# PowerShell automatically sets Type as “Lifecycle” because it is the
# only available option currently
$rule1 = New-AzStorageAccountManagementPolicyRule -Name Test -Action $action -Filter $filter

#Set the policy
$policy = Set-AzStorageAccountManagementPolicy -ResourceGroupName $rgname -StorageAccountName $accountName -Rule $rule1
```