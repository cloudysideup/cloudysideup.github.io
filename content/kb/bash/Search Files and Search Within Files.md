```bash
grep -rni "text" /directory/path
```

```bash
find /directory/path -type f exec grep -l "text" {} \;
```

Thanks [Linux Journal](https://www.linuxjournal.com/content/how-search-and-find-files-text-strings-linux)