# Git Config
### Setting Global Identity

First, since we are starting with a new Git install, we need to set our global identity settings

```
git config --global user.name "David Tucker"
git config --global user.email david@globomantics.com
```

### Setting up An Initial Repository

Next, we will setup an initial repository:

```
mkdir repo
cd repo
git init
echo "# Sample Git Repo" >> README.md
git add .
git commit -m "Initial Commit"
```

### Reviewing the Current Config

From within our sample repository, we will run the following command to view all configuration:

```
git config --list --show-origin
```

This should output the entire config chain from the current repository, to user global settings, to settings for the Git installation. 

### Configuring Multiple Identities 

There are many cases where you will need to use a different identity for a separate repository (for example work versus personal development repo).  In these cases, you can just override your global config values with local ones for identity:

```
git config --local user.name "David Tucker"
git config --local user.email david@davidtucker.net
```

## Setting Standard Config Values

The next step will involve setting common config values to adjust the default functionality of Git.

### Setting the Default Editor

First we'll be updating the default editor for Git to be [VS Code](https://code.visualstudio.com/).  You could just as easily utilize other editors like vim, emacs, Sublime Text, etc...

To set VS Code as the editor, enter the following config command: 

```
git config --global core.editor "code --new-window --wait"
```

You can verify this command, by editing the config file using the following command:
```
git config --global -e
```

### Setting the Default Diff Viewer

From the open config file, we can configure VS Code to also serve as the diff tool by adding the following:

```
[diff]
	tool = vscode
[difftool "vscode"]
	cmd = code --wait --diff $LOCAL $REMOTE
	prompt = false
```

Now, we can test out our diff tool:

```
echo "Sample content" >> README.md
git difftool
```

### Setting the Merge Viewer

Next, we can update our merge tool to be VS Code as well.  We will need to edit our global config to include the following:

```
[merge]
	tool = vscode
[mergetool "vscode"]
	cmd = code --wait $MERGED
```

Once this is in place, we can create a merge conflict:

```
git checkout -b develop
echo "More content." >> README.md
git add .
git commit -m "Updating README.md"
git checkout master
echo "Other content." >> README.md
git add .
git commit -m "Adding other content"
git merge develop
git mergetool
```

We can accept one of the changes in VS Code, and then commit our merge:
```
git commit
```

### Setting Aliases

Git allows you to define aliases for common commands.  In this way, you can reduce the amount of characters you will need to type for things you use regularly.

First we will add an alias for `git status`:

```
git config --global alias.st status
git st
```

Next, we will define another alias for `git status --short`.  You will see that we are reusing the alias in our new definition:

```
git config --global alias.ss 'st --short'
```

Next, we can add in an alias for editing our global config:
```
git config --global alias.egc 'config --global -e'
git egc
```

From within our global config, we can add some additional aliases:

```
[alias]
	st = status
	ss = st --short
	egc = config --global -e
	elc = config --local -e
	esc = config --system -e
	co = checkout
	br = branch
	ci = commit
	amend = co -a --amend
```

We can now save this file.

## Git Attribute
### Excluding Files from Generated Output

We will be leveraging the ability that some Git hosts provide to leverage Git attributes to determine what is included in the generated output of a repository.
```
git add tests
git commit -m "Adding tests directory"
git remote add origin git@github.com:<YOUR REPO URL>
git push -u origin master
```

Now, we'll create a tag for this version:
```
git tag 0.0.1
git push origin 0.0.1
```

Next, we will navigate to Github to see our release.  If we navigate to the releaes section, we will see that the zip for the release includes all files in the repository. Next we will update the `.gitattributes` file with the following:
```
.*				export-ignore
tests			export-ignore
```

Now we will commit and create a new tag:
```
git add .gitattributes
git commit -m "Adding export-ignore attribute"
git tag 0.0.2
git push origin 0.0.2
```

When we navigate to releases now, we should see the zip file contain only the files we want it to contain.

## Using Filters

Next, we will define a clean and smudge filter for our repository that will enable us to keep a secret key locally and not in our remote repository. 

First, we'll paste the following code into our `index.js` file:
```
const API_KEY = "a1693afecca123";

const makeAPICall = function(key) {
    console.log("Making API Call.....");
}

makeAPICall(API_KEY);
```

Then we'll add in the following two config values for our local repository.  This leverages [sed](https://www.geeksforgeeks.org/sed-command-in-linux-unix-with-examples/), a command line tool that transform text.  In this case, we will simply be leveraging it to find and replace a specific string.  
```
git config --local filter.updateAPIKey.smudge 'sed "s/{SECURE_API_KEY}/a1693afecca123/"'
git config --local filter.updateAPIKey.clean 'sed "s/a1693afecca123/{SECURE_API_KEY}/"'
```

Next, we'll paste the following code into our `.gitattributes` file:
```
*.js			filter=updateAPIKey
```

Now, we'll commit our `.gitattributes` file:
```
git add .gitattributes
git commit -m "Adding filter to attributes"
```

Then we'll add in our `index.js` file:
```
git add index.js
git commit -m "Adding updated index.js file"
git push origin master
````
