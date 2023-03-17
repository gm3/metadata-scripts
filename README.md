
```
(Get-Content "CIDs.json" -Raw | ConvertFrom-Json) | ForEach-Object { $_.cid += "/$($_.name)"; $_ } | ConvertTo-Json | Set-Content "output.json"
```
THis will merge the CIDs with the name feild in the array of CiDs



Sort Metadata in natural order
```
(Get-Content -Raw ./file2_updated.json | ConvertFrom-Json) | Sort-Object {[regex]::Replace($_.name, '\d+', { $args[0].Value.PadLeft(20) })} | ConvertTo-Json -Depth 100 | Set-Content ./sorted_file.json
```
