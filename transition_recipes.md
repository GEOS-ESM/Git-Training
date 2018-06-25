During a short window in time, GMAO will be maintaining the CVS repository alongside
the new Git repository. Eventually the CVS repository will become read only, but until
then processes are needed to propagate changes  between the two repositories.


For now, let us assume that some development work has occurred in CVS and the developer
wishes to propagate the changes to the develop/agcm branch in the Git repository.

We will assume that the user already has a CVS working directory `<cvs-dir>` containing the
changes.     

First, we clone the GitHub repository and checkout the develop/agcm branch.    
Be sure to issue these commands somewhere outside the CVS tree:

    # Clone the repository
    git clone git@github.com:GoddardESM/GEOSsystem.git
    cd GEOSsystem
    # Limit the view to the directories in GEOSagcm:
    ./src/set_configuration GEOSagcm
    # Checkout the GEOSagcm branch
    git checkout develop/GEOSagcm
    # Create a new (local) temporary branch for our changes
    git checkout -b feature/$USER/marge-changes-from-cvs

Next we copy all of the changed files from CVS into the corresponding location in Git.

Now we are ready to commit the changed files and push them to a temporary branch on GitHub:

    git add -A
    git commit -m 'a short pithy message'
    git push origin feature/$USER/merge-changes-from-cvs

The changes should now all be in a temporary branch on GitHub, so all that remains is
to create a pull-request merging onto develop/agcm from out temporary branch.    The code owners
for that branch will be notified and can accept the changes after the automated tests pass.


    
    



