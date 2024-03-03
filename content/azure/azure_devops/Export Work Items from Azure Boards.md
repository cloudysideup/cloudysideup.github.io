```PowerShell
$witlinktype = (az boards work-item show --org $orgURL --id $witID --query "relations[].attributes[].name" | convertfrom-json)
$linkedwiturls = (az boards work-item show --org $orgURL --id $witID --query "relations[].url" | convertfrom-json)

$workitemlinks = for ($i=0; $i -lt $witlinktype.Count; $i++) {
[PSCustomObject]@{
WITLinkType = $witlinktype[$i]
WorkItem = $linkedwiturls[$i]
}
}
$workitemlinks
```