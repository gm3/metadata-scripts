## Download the CIDs from web3storage
Use web3STorage HTTP request to get the data

## Removing the data other than the CIDS and filenames in Powershell
After uploading data to web3storage i used HTTP request to download the metadata, and used this script to concatinate everything but the names and CIDs

```
(Get-Content "file.json" -Raw | ConvertFrom-Json).ForEach({ '{ "name": "' + $_.name + '", "cid": "' + $_.cid + '" }' }) | Set-Content "output.json"
```
Here's how it works:

* Get-Content cmdlet reads the contents of the input JSON file and outputs it as a string.
* ConvertFrom-Json cmdlet converts the string into a PowerShell object.
* The ForEach method is called on the resulting PowerShell object, which loops through each object in the object.
* A string is created for each object that contains only the name and cid attributes and values, using single quotes to wrap the string.
* The resulting strings are wrapped in braces to create a JSON object.
* The output is written to the output file using Set-Content cmdlet.
* This should output the concatenated JSON objects with only the name and cid attributes and values for each entry in the input JSON file, written one after the other into the output file.

---


```
(Get-Content "CIDs.json" -Raw | ConvertFrom-Json) | ForEach-Object { $_.cid += "/$($_.name)"; $_ } | ConvertTo-Json | Set-Content "output.json"
```
THis will merge the CIDs with the name feild in the array of CiDs



## Sort Metadata in natural order
```
(Get-Content -Raw ./file2_updated.json | ConvertFrom-Json) | Sort-Object {[regex]::Replace($_.name, '\d+', { $args[0].Value.PadLeft(20) })} | ConvertTo-Json -Depth 100 | Set-Content ./sorted_file.json
```

This will sort metadata in natural order 1,2,3,4,5,6,7,8,9,10 etc

## Add filename to CID URL in json file
```
(Get-Content "input.json" -Raw | ConvertFrom-Json) | ForEach-Object { $_.cid += "/$($_.name)"; $_ } | ConvertTo-Json | Set-Content "output.json"
```


## Using Regex to replace the cid with animation_url for glbs

You can use the Visual Studio Code search and replace functionality with regular expressions to achieve this.

* Open the file containing the JSON data in Visual Studio Code.
* Press Ctrl+Shift+F (Windows/Linux) or Cmd+Shift+F (macOS) to open the search panel.
* Click the .* button on the right side of the search field to enable regular expression mode.
* Enter the following regular expression in the search field: `("cid":\s*")([^"]*\.glb)`
* This pattern matches any "cid" key followed by a .glb file path.
* Enter the following regular expression in the replace field: "animation_url": "$2"
* This replaces the "cid" key with "animation_url" and preserves the .glb file path in the value.
* Click the "Replace All" button to replace all instances of "cid" with "animation_url" for .glb files in the document.

you can then do this for images (jpg) and the vrm_url(vrm)

---

## Removing the .glb extention from the name using RegEX in VIsual Studio Code

You can use the following regex pattern in Visual Studio Code to remove the .glb extension only from the value of the name property:

Find: `("name":\s*"[^"]+)\.glb(")`

Replace: `$1$2`

This pattern matches the name property followed by the .glb extension, captures the text before the extension in group 1, and replaces the entire match with group 1 and group 2, effectively removing the extension.

Make sure to enable the regex search mode in Visual Studio Code by pressing Ctrl + Shift + F or Cmd + Shift + F and selecting the .* icon on the right side of the search box. Also, remember to double-check the changes before saving the file.

---

## Merge JSON with CIDs

To merge the CID json file and the Metadata based on the name attribute, use this script

```
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process
```
Set the policy to be able to execute scripts
```
$file1Path = 'file1.json'
$file2Path = 'file2.json'
$outputPath = 'file2_updated.json'

$file1Content = Get-Content $file1Path | ConvertFrom-Json
$file2Content = Get-Content $file2Path | ConvertFrom-Json

foreach ($item2 in $file2Content) {
    $matchingItem = $file1Content | Where-Object { $_.name -eq $item2.name }
    if ($matchingItem) {
        $item2.vrm_url = $matchingItem.vrm_url
        $item2.animation_url = $matchingItem.animation_url
        $item2.image = $matchingItem.image
    }
}

$file2Content | ConvertTo-Json -Depth 100 | Set-Content $outputPath
```

`.\replaceURls.ps1`

