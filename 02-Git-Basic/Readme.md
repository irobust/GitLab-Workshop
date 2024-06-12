# Git Basic Command
## Basic command
### Setting up An Initial Repository
```
mkdir repo
cd repo
git init
echo "# Sample Git Repo" >> README.md
git add .
git commit -m "Initial Commit"
```

### Managing file in three stages
```
git diff
git diff --cached
git diff HEAD
git diff -w
git checkout
git restore .
git checkout HEAD [file-or-path]
git restore --source=HEAD --staged [file-or-path]
git rm --cached [file-or-path]
git mv [filename] [filename]
```

### Reset and Revert
```
git reset --hard HEAD
git reset --hard [commit]
git reset --mixed [commit]
git reset --soft [commit]
git reset HEAD [file-or-path]
git revert [commit]
```

### Working with Stash
```
git stash --include-untracked
git stash list
git stash apply
git stash apply [stash-name]
git stash clear
```

### Explore the history
```
git log --graph --decorate —-oneline
git log --patch
git log --grep [text]
git log -G"regular expression" --patch
git log -3 --oneline
git log HEAD~5..HEAD^
git log --since=2.minutes.ago
git log --since="2022-10-01" --until="2022-10-05"
git log branch-name..master --oneline
git show HEAD^
git show HEAD~2
git show HEAD@{"1 month ago"}
git diff [commit]
git diff --cached [commit]
git diff [commit] [commit]
git diff HEAD..HEAD^^
git blame recipes/apple_pie.txt
git show HEAD
```

### Git Tag
```
git tag [tagname] [commit] # lightweight tag (soft)
git tag -a [tagname] -m "add some message here" [commit] # annotated tag (hard)
git tag
git tag -l "v1.0*"
git tag -n   # show firstline of annotation or commit message(lightweight)
git tag -n5   # five lines
git show [tagname]   # show commit hash
git diff v1.0..v1.1
git push origin [tagname]
git push origin --tags
```

### Fixing Mistake
```
git commit --amend
git commit --amend -m "add some message here"
git rebase -i
git rebase -i origin/[branch-name]
git rebase --continue
git reflog show --all
git reflog show HEAD
git reflog refs/head/master
```
