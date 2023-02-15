# useful-things

## Checking JSON For duplicates

### Bash script to ject check the atributes in a folder of JSON files for duplicates
This script uses the jq command to extract only the "attributes" array from each JSON file and pass it through a pipeline to generate an MD5 checksum. The sort and uniq commands are then used to identify duplicate checksums, and the cut and xargs commands are used to delete the duplicate files.

Note that you will need to have the jq command installed for this script to work. You can install it using your package manager (e.g. apt-get install jq on Debian/Ubuntu).

```bash
cd /path/to/directory/

for file in *.json; do
    cat "$file" | jq '.attributes' | sort | md5sum >> checksums.txt
done

sort checksums.txt | uniq -d | cut -d' ' -f3- | xargs rm

rm checksums.txt
```

---


### Powershell Duplicate Check

This PowerShell script works similarly to the earlier Python script. It loops through each JSON file in the directory, extracts the "attributes" array using ConvertFrom-Json and ConvertTo-Json, generates an MD5 hash of the attributes using Get-FileHash, and checks for duplicates using a hashtable.

Note that this script assumes that each JSON file has the same structure as the example JSON you provided earlier, with an "attributes" array containing the trait types and values. If the structure of the JSON files is different, you may need to modify the script accordingly.

```perl
$dir = "C:\path\to\directory"
$files = Get-ChildItem -Path $dir -Filter *.json

$hashes = @{}
$duplicates = @()

foreach ($file in $files) {
    $json = Get-Content $file.FullName | ConvertFrom-Json
    $attributes = $json.attributes | ConvertTo-Json -Compress
    $hash = Get-FileHash $file.FullName -Algorithm MD5
    if ($hashes.ContainsKey($hash.Hash)) {
        $duplicates += $file.FullName
    } else {
        $hashes.Add($hash.Hash, $attributes)
    }
}

if ($duplicates.Count -gt 0) {
    Write-Host "Duplicates found:"
    $duplicates | ForEach-Object { Write-Host "  $_" }
} else {
    Write-Host "No duplicates found."
}
```
