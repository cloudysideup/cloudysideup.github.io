[Charbel Nemnom](https://charbelnemnom.com) put together a script to get a list of all the network security groups (NSGs) in all Azure Subscriptions.  I modified it to get all the subnets (with or without NSGs) instead.

```powershell
<#
.Synopsis
A script used to export subnet information in all your Azure subscriptions.

.Description
This script gets a list of all subnets within a virtual network for all Azure subscriptions you can access.
It exports the report into a CSV file in Azure Cloud Shell or locally.

.Notes
Created : 29-Nov-2023
Updated : 29-Nov-2023
Version : 0.2
Author : Jason Elliott
Digital Garden : https://cloudysideup.com
Disclaimer: This script is provided "AS IS" with no warranties.
#>

##TODO: Remove Duplicates in list object
##TODO: Sort by vnet name then subnet name

#! Install Az Module If Needed
function Install-Module-If-Needed {
  param([string]$ModuleName)
  if (Get-Module -ListAvailable -Name $ModuleName -Verbose:$false) {
    Write-Host `
    "Module '$($ModuleName)' already exists, continue..." `
    -ForegroundColor Green
  }
  else {
    Write-Host `
    "Module '$($ModuleName)' does not exist, installing..." `
    -ForegroundColor Yellow
    Install-Module $ModuleName -Force -AllowClobber -ErrorAction Stop
    Write-Host "Module '$($ModuleName)' installed." -ForegroundColor Green
  }
}

#! Install Az Accounts Module If Needed
Install-Module-If-Needed Az.Accounts

#! Install Az Network Module If Needed
Install-Module-If-Needed Az.Network

#! Check Azure Connection
Try {
  Write-Verbose "Connecting to Azure Cloud..."
  Connect-AzAccount -ErrorAction Stop -WarningAction SilentlyContinue | Out-Null
}
Catch {
  Write-Warning `
  "Cannot connect to Azure Cloud. Please check your credentials. Exiting!"
  Break
}

#! Get all Azure Subscriptions
$azSubs = Get-AzSubscription

#! Use the following if you want to select a specific Azure Subscription
#$azSubs = Get-AzSubscription | Out-Gridview -PassThru -Title 'Select Azure Subscription'

foreach ( $azSub in $azSubs ) {
  Set-AzContext -Subscription $azSub | Out-Null
  $azSubName = $azSub.Name
  $list = $null
  $list = @()
  $vnets = Get-AzVirtualNetwork

  foreach ($vnet in $vnets) {
    $subnets = $vnet.subnets.name
    foreach ($subnet in $subnets) { 
      $SubnetDetails = Get-AzVirtualNetworkSubnetConfig `
      -Name $subnet -VirtualNetwork $vnet
      if ($SubnetDetails.NetworkSecurityGroup -eq $Null) {
        $list += [PSCustomObject]@{
          VNETName = $vnet.Name
          VNETAddressSpaces = $vnet.AddressSpace.AddressPrefixes -join ', '
          SubnetName = $SubnetDetails.name
          SubnetsPrefix = $SubnetDetails.AddressPrefix -join ''
          SubnetNSG = "No NSG"
          SubnetNSGID = "No NSG"
        }
      }
      else {
        $list += [PSCustomObject]@{
          VNETName = $vnet.Name
          VNETAddressSpaces = $vnet.AddressSpace.AddressPrefixes -join ', '
          SubnetName = $SubnetDetails.name
          SubnetsPrefix = $SubnetDetails.AddressPrefix -join ''
          SubnetNSG = $SubnetDetails.NetworkSecurityGroup.id.split('/')[8]
          SubnetNSGID = $SubnetDetails.NetworkSecurityGroup.id
        }
      }
    }
    $list | `
    Export-Csv `
    -Path "$($home)\clouddrive\$azSubName-subnets.csv" `
    -NoTypeInformation -Append -force
  }
}
    # to export local
    # Export-Csv -Path=".\$azSubName-subnets.csv" -NoTypeInformation -Append -force
``` 
