# Git Sub Modules
## Adding a Submodule to a Project

In this clip, we will verify that we can add a submodule, update our submodule, configure Git for submodules, and delete a submodule.

## Adding a Repository

First, we will create a sample repository to use for this clip.

```
mkdir repo
cd repo
git init
echo "# Git Test Repository" >> README.md
git add .
git commit -m "Initial commit"
mkdir external
```

Next, we will add in an example submodule within our repository:
```
git submodule add https://github.com/davidtucker/example-submodule.git external/example-submodule
```

If we navigate to our submodule, we will see the contents of the repository included in the directory that we specified.
```
cd external/example-submodule
ls
cd ../..
```

We can also see the contents of the new file `.gitmodules` file. 
```
cat .gitmodules
```

### Git Configuration for Submodules

Next, we can see the current state of the repository by running `git status`:
```
git status
```

We can make a change to make this information even more useful for us when working with submodules:
```
git config --global status.submoduleSummary true
```

Now, we can see the updated information using `git status`:
```
git status
```

Next, we can commit to our repository:
```
git add .
```
```
git commit -m "Adding example submodule"
```

In addition, we can improve diffs when working with submodules:
```
git config --global diff.submodule log
```

### Cloning a Repository with a Submodule

Next, we will clone the repository we were just working with using the file protocol:
```
cd ..
```

```
git clone repo clone
```

Now, we can view the contents of our submodule within the cloned repository:
```
cd clone
ls
cd external/example-submodule
cd ../../
```

We will notice that our submodule is not yet populated.  We need to first run the following to get the info from the `.gitmodules` file into our repository config:
```
git submodule init
```

Next, we need to tell the submodule to grab the contents of its repository:
```
git submodule update
```

### Updating the Reference to Our Submodule

One of the benefits of using a submodule, is that you can target a specific commit of the submodule and ensure that you will work with that specific commit until you (or a member of your team) specifically reference another commit.

First, we need to `fetch` content for our repository:
```
cd external/example-submodule
git fetch
```

Then we can checkout the correct branch:
```
git checkout beta
```

Next, we can navigate back into our repository:
```
cd ../../
```

From here, we can see the repository status:
```
git status
```

Next, we can commit this change:
```
git commit -am "Updating submodule to beta"
```

Then we need to run submodule update:
```
git submodule update
```

### Removing a Submodule

Next, we can permanently remove a submodule from our project. First, we want to `deinit` the module, which will remove it from our repository config:

```
git submodule deinit external/example-submodule
```

Next, we are ready to permanently delete the reference to the submodule:
```
git rm external/example-submodule
```

Next, we can see the status for our repository:
```
git status
```

Now, we can commit the removal:
```
git commit -am "Removing submodule"
```

## Nested Submodules

If you are using submodules on a project, it will be rare to not run across nested submodules (submodules within submodules).  If we follow the code we used in the previous clip, we would have to navigate to each submodule directory and both `init` and `update` it.  Fortunately, Git provides another way.

If you are leveraging a repository that leverages nested repositories, you can use the `--recursive` option:
```
git clone --recursive https://github.com/davidtucker/example-nested-submodule.git repo
cd repo
```

This is the equivalent of cloning, init'ing, and updating each submodule recursively.

If we navigate to this repository, we can see that each of the three submodules have been populated.  We can also see them in the `gitmodules` file:
```
cat .gitmodules
```

We can now use another command `git submodule foreach` to see the `.gitmodules` file for each of the submodules:
```
git submodule foreach 'cat .gitmodules'
```

### Working with Teams

Submodules can introduce challenges to a team workflow if specific guidelines are not followed.

#### Pulling from a Repo with Submodules

First, we will need to pull code from the main module just as we normally would:
```
git pull
```

In some cases, our Git submodule configuration, found in `.gitmodules` can be changed by one of our team members.  For example, there could be the potential that a developer chose to fork a repository and use the URL for the forked repository instead of the previous one.  In these cases, you need to sync your configuration with what is contained in the `.gitmodules` file. 
```
git submodule sync --recursive
```

Next, you will need to update your submodules.  However, we should also remember that some repositories may be new since we just pulled down code from our remote.  In addition, this could happen in any of our submodules or any of their submodules.  We can handle all of these scenarios with the following command:
```
git submodule update --init --recursive
```

### Working within the Submodules

There are multiple problems that can occur when working with our submodules.  The primary challenge occurs when we push a reference to a commit for a submodule that we haven't yet pushed to the remote.  In this situation, we have pretty much broken our teammate's reference to the submodule.  
```
cd external/repo1/
git checkout master
git pull --rebase
echo "Editing a submodule" >> README.md
git commit -am "Updating the README"
cd ../../
git commit -am "Updating our submodule code"
```

If we pushed our code to the remote from here, it would cause the problem we referred to earlier.  Git now provides a config option (since Git 2.7) to prevent this from happening in many cases.  If you set `pull.recurseSubmodules` it will check to be sure that the first level of submodules is checked for any commits that need to be pushed when we push the primary repository. We'll go ahead and set that value here:
```
git config --global push.recurseSubmodules on-demand
```

Now, when we push our primary repository, we should see our submodule push too.
```
git push
```