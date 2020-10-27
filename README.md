Setting Blob Tiers to 'Archive' for large data sets in Azure Blob Storage
=========================================================================

            

I created this script for a client who wanted to change their off-site backups from Tapes to Azure Blob storage. The total amount of data to be migrated to Azure was approximately 40TB and for this to align appropriately with their on-premise storage structure,
 we would spread this across 6 Blob containers. This off-site data would only need to be accessed in a DR scenario, and there are no incremental file changes, just nightly uploads of new files. So to get the best value we decided that all data uploaded should
 be set in the 'Archive' Blob Access Tier.

Currently (March 2018) there is no option within the Storage service in Azure to create a 'Archive' access tier storage account. My only option was to create a 'Hot' or 'Cold' tier storage account, and subsequent containers, and then individually set each blob
 (file) within the containers to the 'Archive' Tier. Through some online recommendations, I found that the best option was to migrate all data into a Hot storage account/container and then change each file to the 'Archive' access tier. The reason for this is
 that you are not charged for moving blobs from the 'Hot' tier, but you are charged for moving blobs from the 'Cold' tier.


I started uploading the on-premise files to the 'Hot' storage account using the
**AZCOPY** utility, which worked as expected.


Below where my requirements


  *  Once files were uploaded to the corresponding Azure Blob storage container, run a script that will change the Access Tier for each blob from 'Hot' to 'Archive'

  *  I wanted to be emailed a report of blobs when they had there Access Tier changed. Just to confirm the script was running as expected and on schedule


So I started to develop my script, which pretty much revolved around the **Get-AzureStorageBlob** cmdlet! As the size of the containers grew, I soon realised that the SHELL and cmdlet where unable to process large amount of datasets at
 one time, so I came across an article describing the use of the **Continuation Token** within the **Get-AzureStorageBlob** cmdlet. This allows for blobs to be pulled down in smaller batches or sub-sets to be processed incrementally.


I ran this script every hour during the initial migration phase, and now every night on each Blob container.


I'm not the most savvy powershell scripter, so you'll probably come across some unnecessary bulk in my script! But I hope parts of it can be useful to others in the community and any feedback, positive or negative, would be greatly appreciated.


Thanks


Brendan


 

 

        
    
TechNet gallery is retiring! This script was migrated from TechNet script center to GitHub by Microsoft Azure Automation product group. All the Script Center fields like Rating, RatingCount and DownloadCount have been carried over to Github as-is for the migrated scripts only. Note : The Script Center fields will not be applicable for the new repositories created in Github & hence those fields will not show up for new Github repositories.
