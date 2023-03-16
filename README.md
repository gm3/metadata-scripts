
(Get-Content "CIDs.json" -Raw | ConvertFrom-Json) | ForEach-Object { $_.cid += "/$($_.name)"; $_ } | ConvertTo-Json | Set-Content "output.json"

THis will merge the CIDs with the name feild in the array of CiDs
