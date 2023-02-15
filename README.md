# useful-things

```bash
cd /path/to/directory/

for file in *.json; do
    cat "$file" | jq '.attributes' | sort | md5sum >> checksums.txt
done

sort checksums.txt | uniq -d | cut -d' ' -f3- | xargs rm

rm checksums.txt
```

This script uses the jq command to extract only the "attributes" array from each JSON file and pass it through a pipeline to generate an MD5 checksum. The sort and uniq commands are then used to identify duplicate checksums, and the cut and xargs commands are used to delete the duplicate files.

Note that you will need to have the jq command installed for this script to work. You can install it using your package manager (e.g. apt-get install jq on Debian/Ubuntu).
