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



Sort Metadata in natural order
```
(Get-Content -Raw ./file2_updated.json | ConvertFrom-Json) | Sort-Object {[regex]::Replace($_.name, '\d+', { $args[0].Value.PadLeft(20) })} | ConvertTo-Json -Depth 100 | Set-Content ./sorted_file.json
```
