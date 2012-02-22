To move all .avi files from a given dir (including subdirs!)to another dir:

```bash
find . -name "*.avi" -print | while read file ; do
mv "$file" /media/nas/data/target
done
```