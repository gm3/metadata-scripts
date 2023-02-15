# useful-things

```bash
cd /path/to/directory/

for file in *.json; do
    cat "$file" | jq '.attributes' | sort | md5sum >> checksums.txt
done

sort checksums.txt | uniq -d | cut -d' ' -f3- | xargs rm

rm checksums.txt
```

This is a bash script to chec for duplicates in JSON attributes
