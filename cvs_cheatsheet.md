# CVS to Git Cheatsheet

## Environment

###  Required git config
Before doing anything, you should prepare your git environment. In git, this is managed through `git config`. In git you have a *global* config that affects every repository on that system, and you can have a per-repository one that supersedes the global configuration. In nearly all cases, one edits/refers to the global config when it comes to the basic git configuration.

The git global config is accessed on the command line by `git config --global`. As an example, we will set the two ***required*** config items--name and email address:
```gitconfig
$ git config --global user.name "Jane Doe"
$ git config --global user.email janedoe@nasa.gov
```
Internally, what this does is edit a file (`$HOME/.gitconfig` on Linux and macOS), which will now have:
```gitconfig
[user]
        name = Jane Doe
        email = janedoe@nasa.gov
```
These are ***required*** because these are what Git uses to "sign" commits.
### Editor
Another useful configuration setting is that of the editor. When you commit some code, like when you do `cvs commit` an editor will appear for you to write your commit message. Git determines this editor in three ways. First, git will respect the `EDITOR` environment variable. Second, if set, git will defer to the `GIT_EDITOR` environment variable. Finally, you can set an editor through `git config` with: `git config --global core.editor`. So, these are all equivalent:

* `setenv EDITOR=vim`
* `setenv GIT_EDITOR=vim`
* `git config --global core.editor vim`

where `vim` is the example application used, but anything (`emacs`, Sublime Text, etc.) can by used.

## CVS v Git Commands
### Quick Table
|Action|CVS Command| Git Command  |
|--|--|--|
|Checkout the repository|`cvs checkout repo`|`git clone repo`|
|Checkout a branch of the repo|`cvs checkout -r branch repo`|`git clone -b branch repo`|
|Check the status|`cvs status`|`git status`|
|Update the code checkout|`cvs update`|`git remote update`<br>`git pull`|
|Commit changes|`cvs commit`|`git add`<br>`git commit`<br>(*optional*) `git push`|
|Create a branch|`cvs tag -b branch-tag`|`git branch branchName`|
|Update to a branch|`cvs update -r branch-tag`|`git checkout branchName`|
|Create and move to branch in one command|*no equivalent*|`git checkout -b branchName`|
|Tag code|`cvs tag tagName`|`git tag tagName` (*don't do this, see below*)|
|Annotated tag code|*no equivalent*|`git tag -a tagName`|
|Merge code from branch|`cvs update -j remoteBranch`|`git merge remoteBranch`|

#### Remote-focused v Locally-focused

One thing observers will note from above is that git often uses more commands than CVS. The main reason for this is that CVS is "remote-focused" in that nearly everything you do in CVS is automatically pushed to the remote server. If you tag code locally, you tag it on the server at the same time. If you update code, the server automatically gets the latest code on your branch and updates you to it. Indeed, CVS has r-variants of its subcommands that can do things only on a remote.

Git, however, is more "locally-focused". Once you clone a repository, you can tag, commit, branch, merge without ever needing access to the remote repository. Thus, more commands are needed to tell git that you want your local changes pushed to the remote.

### Getting your code
In CVS, one does a `cvs checkout` of a repository. In git, this is done via `git clone`, ***not*** `git checkout`.  In CVS, if you just do `cvs checkout repo` this will get you the HEAD of trunk. In git, `git clone repo-URL` will get you the HEAD of the default line of development which is usually called `master`. 

However, sometimes you want to directly get a branch of the repository. In CVS this is done via `cvs checkout -r branch repoName` while in 

#### CVSROOT and Checkout
For GEOS, all of the CVS code is held in one repository on CVSACL. So to checkout code, one could do:
```
cvs -d :ext:user@cvsacldirect:/cvsroot/esma checkout GEOSagcm
```
However, we usually tell users to set an environment variable, `CVSROOT` so that `cvs checkout` without `-d` will use a default:
```
export CVSROOT :ext:user@cvsacldirect:/cvsroot/esma
```
One can still checkout repos on other servers with `cvs -d`, but a default `cvs checkout` will go from `CVSROOT`.  Git has no equivalent of this. ***Every*** `git clone` must be followed by a URL.

#### Recursive Git Cloning
Many git repositories use what are called *git submodules* to handle *external* codes that are maintained in other repositories. Think of them as "symlinks" to these repos. With out a special flag to `git clone` or a separate command, a clone will not "sub-clone" these submodules, but instead an "empty" (really there is a dot-file) folder will appear. In order to clone and get these submodules one can do a couple of things. First, if you did a `git clone` and realize there is a submodule you can do:
```
git clone repoURL
git submodule init
git submodule update
```
However, git can do this in one step with:
```
git clone --recurse-submodules repoURL
```
### Updating your code
In CVS, to get new code on the branch one is on, one does a `cvs update`. In git, since remote commands are separate from local commands, the equivalent is *three* git tasks that are usually done in *two* commands. First, a git user does:
```
git remote update
```
which says "check the remote for any updates to the branch I'm on". Next, a user says:
```
git pull
```
which is actually two other commands. It does a `git fetch` which brings the remote changes to the local repository database and then `git merge` which merges those changes into the current checkout.

### Committing your code

   # CVS initial checkout                       # Git equivalent
    
    setenv CVSROOT ...                          git clone git@github.com:GoddardESM/GEOSsystem.git  GEOSagcm
    cvs checkout GEOSagcm                       cd GEOSagcm
                                                src/set_configuration.sh GEOSagcm
                                                git checkout GEOSagcm
                                                
                                                # or 
                                                
                                                ggit GEOSagcm


    # CVS add new file                          # Git equivalent
    cvs add [-m message] files                  git add files


    # CVS 
                                                
                                                
                                                
    
                                                
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkzOTY4MjIwMCwzNzk5NTI4MTIsLTEwMj
Q2MTg4MjhdfQ==
-->