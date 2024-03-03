```PowerShell
#
# Azure Pipelines Agent Install & Configure Script (Windows 7 compatible)
#

$path32 = "$HOME\Downloads\vsts-agent-win-x86-2.172.2.zip"
$path64 = "$HOME\Downloads\vsts-agent-win-x64-2.172.2.zip"
$url32 = "https://vstsagentpackage.azureedge.net/agent/2.172.2/vsts-agent-win-x86-2.172.2.zip"
$url64 = "https://vstsagentpackage.azureedge.net/agent/2.172.2/vsts-agent-win-x64-2.172.2.zip"


# Enter information required for agent configuration
$AgentInstallDir = (Read-Host -Prompt "Enter Drive Letter For Install: ") + ":\AzurePipelinesAgent"
$urlADO = "https://dev.azure.com/" + (Read-Host -Prompt "Enter Azure DevOps Organization name: ")
$teamName = (Read-Host -prompt "Enter team abbreviation (no spaces): ")
$agentName = $teamName + (Read-Host -Prompt "Enter Build Server Hostname: ")
$poolName = (Read-Host -Prompt "Enter Agent Pool Name: ")
$token = Read-Host -Prompt "Enter or Paste Personal Access Token (not expired) with Scope of Access to Read & Manage Agent Pool: " -AsSecureString

# Get .Net Full Version
$dotNetVersion = `
Get-ChildItem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse |
Get-ItemProperty -name Version, Release -EA 0 |
Where-Object { $_.PSChildName -eq "Full"} |
Select-Object Release -expandproperty Release

# Check that we're running from an elevated PowerShell window
Write-Host "Checking for elevated permissions..."
If (-NOT ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole(`
[Security.Principal.WindowsBuiltInRole] "Administrator")) {
Write-Warning "Insufficient permissions to run this script. Open the PowerShell console as an administrator and run this script again."
Break
}
Else {
Write-Host "Checking Windows Version..." -ForegroundColor Green
}

# Version of Windows (Windows 7, 8.1, 10, Server 2008 R2 SP1 or higher)
If ([System.Environment]::OSVersion.Version.Major -lt 6) {
  Write-Warning "Windows 7/Server 2008 R2 SP1 or higher required"
  Break
}
Else {
Write-Host "Checking PowerShell Version..." -ForegroundColor Green
}

# PowerShell 3.0 or higher
If ($PSVersionTable.PSVersion.Major -lt 3) {
  Write-Warning "PowerShell 3.0 or higher required"
  Break
}
Else {
Write-Host "Checking .Net Framework Version..." -ForegroundColor Green
}

# .NET Framework 4.6.2 or higher
If ($dotNetVersion -lt 394802) {
  Write-Warning ".Net Framework 4.6.2 or higher required"
  Break
}

Else {
Write-Host "Installing and Configuring Azure Pipelines Agent..."
}

# download, extract and install Azure Pipelines Agent
Try {
  New-Item -ItemType "directory" -Path $AgentInstallDir
  Set-Location $AgentInstallDir
  $client = New-Object System.Net.WebClient
  If((Get-CimInStance Win32_OperatingSystem).OSArchitecture.Trim() -eq "64-Bit") {
    $client.DownloadFile($url64, $path64)
    Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\Downloads\vsts-agent-win-x64-2.172.2.zip", "$PWD")
    .\config.cmd --unattended --url $urlADO --auth pat --token [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($token)) --pool $poolName --agent $agentName
  }
  Else {
    $client.DownloadFile($url32, $path32)
    Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\Downloads\vsts-agent-win-x86-2.172.2.zip", "$PWD")
    .\config.cmd --unattended --url $urlADO --auth pat --token [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($token)) --pool $poolName --agent $agentName
  }
}
Catch {
Add-Content c:\Azure_Pipelines_Agent_Install_Fails.txt"`n$ENV:computername"
} 
```