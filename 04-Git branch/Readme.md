# Git Branch
### Initialize directory as git repo
1. Add original bug.txt to the directory
```
This was supposed to be a simple fix
It's not
This line belongs in a file called "function1.txt"
This line should be deleted because it's old
This line belongs in a file called "function2.txt"
Here is the actual problem
```
Commit
```
git init
git add bug.txt
git commit -m "Demo setup"
```

2. Perform bugfixes (delete line, fix typo, move 2 lines, create 2 new files) and show status
```
git status
```
3. Create a branch called "quickfix" and switch to that branch
```
git checkout -b quickfix
```
4. Add and commit all changes to that branch
```
git add *
git status
git commit -m "quickfix not so quick"
```
5. Switch back to main
```
git checkout main
```

6. Notice that main is in the original state.
7. Switch back to quickfix to continue working on the bug
```
git checkout quickfix
```

