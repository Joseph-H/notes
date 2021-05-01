Discard remote changes and mark file as resolved
git checkout --ours . (can also be git checkout --ours -- <filename>) - checkout our local version of all files (the dot at the end is important, don't forget it) (you can also do git checkout --theirs -- <filename>) to ignore your file and get remote changes
git add -u - mark all conflicted files as merged
git commit

Explanation for the dot:
git checkout has two modes; one in which it switches branches, and one in which it checks files out of the index into the working copy (sometimes pulling them into the index from another revision first). The way it distinguishes is by whether you've passed a filename in; if you haven't passed in a filename, it tries switching branches (though if you don't pass in a branch either, it will just try checking out the current branch again), but it refuses to do so if there are modified files that that would affect. So, if you want a behavior that will overwrite existing files, you need to pass in . or a filename in order to get the second behavior from git checkout.

Explanation for git staging area:
In the index (also known as the "staging area"; the place where files are stored by git add before committing them), you will have 3 versions of each file with conflicts; there is the original version of the file from the ancestor of the two branches you are merging, the version from HEAD (your side of the merge), and the version from the remote branch.
In order to resolve the conflict, you can either edit the file that is in your working copy, removing the conflict markers and fixing the code up so that it works. Or, you can check out the version from one or the other sides of the merge, using git checkout --ours or git checkout --theirs. Once you have put the file into the state you want it, you indicate that you are done merging the file and it is ready to commit using git add, and then you can commit the merge with git commit.

Make master take whatever you have in your branch
Checkout your branch
git merge -s ours master
Push your changes

Roll back changes to a certain commit
git checkout 2196ddf75d017c50dbd71000586f8eef7398eb24 (you will now have a detached head)
git checkout -b feature/BISON... (create a branch off this commit)
Push your changes

Abort a merge
git merge --abort