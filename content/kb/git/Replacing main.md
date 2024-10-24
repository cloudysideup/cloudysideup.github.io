Yes, it's an anti-pattern. For a repo with one contributor, meh.

```bash
git checkout main
git reset --hard <better_branch>
git push -f origin main
```

Thanks to [Brandon Howard](https://stackoverflow.com/a/31036317)
