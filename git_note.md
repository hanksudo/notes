## Common instruction

```bash
# download repo
git clone <url>
# switch branch
git checkout <branch>
# commit changes
git commit

# sync change to repo
git pull
git push

git status
git show
git show 97d3cfd

git blame path/to/file.py

git cherrypick <commit>
git reset --hard HEAD^

git checkout <feature>
git rebase master
git push -f
git mergetool

git reflog

git commit --amend
git rebase --interactive
git rebase --i
git rebase --interactive HEAD~5

# remove local and remote tag
git tag -d tag_name
git push origin :refs/tags/tag_name
```

## Force master update to upstream

```
git remote add upstream /url/to/original/repo
git fetch upstream
git checkout master
git reset --hard upstream/master  
git push origin master --force 
```

### Create and apply patch (with images)

```bash
git diff-index 06e5c52 --binary > patch.diff
git apply patch.diff
```