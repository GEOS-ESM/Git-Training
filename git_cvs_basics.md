# 

Some basic differences between Git and CVS.

CVS is a software system that allows users to coordinate software development between a local "working directory" and a *centralized* repository.    The repository contains (at least conceptually) every version of every file that has ever been committed, while the working directory is a slice of specific revisions for each file.   Virtually all CVS commands therefore require access to the central repository.

Git uses what is known as a distributed repository approach.   Here, instead of working with a snapshot, workers actually retrieve an entire copy ("clone") of the repository.    Because the entire history of the repository is available locally, many/most Git commands do *not* require access to the original repository.   This encourages developers to make frequent small commits in their local clone.     The distributed repository approach also enables developers to exchange code changes in a peer-to-peer fashion with the local clone being "aware" of multiple copies of the repository.   However, in practice, most efforts designate a single main repository that serves for coordination among separate development efforts.

One important conceptual difference between Git and CVS that often confuses newcomers is that Git generally works in a more *holistic* manner than CVS.  In particular, Git tags and branches apply to the entire software tree and not to individual files.    Whereas in CVS there is little difference between committing a sequence of files individually and a single commit that includes the entire list of files, in Git developers are strongly recommended to committ all related changes in a single command.    In theory this means that each commit is in-and-of-itself more consistent, making it harder for instance for another developer to pull in some change that updates an interface without also pulling in the updates to the *uses* of the interface.

Another major difference between Git and CVS is that Git has no analog for CVS *modules*.   CVS modules allow developers to checkout different subsets of the repository and even provides the means to arrange the subdirectories to be relocated and renamed under different modules.  The closest analog that Git provides is the ability to do a sparse checkout from the repository.   This allows a developer to prune subtrees from their working directory, but does not support wholesale rearrangement of directories.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYyMTY5MTAzOV19
-->