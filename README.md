# git-merge-conflict-resolve

This repository is meant to practise the process of resolving merge conflicts

fork this repository and follow the steps below.

# Resolving Merge Conflicts between your branch and another branch
[ref](https://www.atlassian.com/git/tutorials/using-branches/merge-conflicts)


## Creating a merge conflict

### Lets first add a file to our `main` branch which we will make changes to in future branches.

    ```
    $ echo "this is some content to mess with" > merge.txt
    $ git add merge.txt
    $ git commit -am "we are commiting the inital content"
    $ git push
    ```

### Now lets suppose you create a branch off of the main branch and want to make additional changes to the merge.txt file which was created on the `main` branch, i.e. call the new branch `new_branch_to_merge_later`

    ```BASH
    $ git checkout -b new_branch_to_merge_later
    $ echo "totally different content to merge later" > merge.txt
    $ git commit -am"edited the content of merge.txt to cause a conflict"
    $ git push --set-upstream origin new_branch_to_merge_later
    ```

### In real life there may have been other people that may have been updating the main branch as they merged their branches with the main branch. We will go back to the main branch to create some changes to simulate this situation
    
    i.e. 
    ```BASH
    $ git checkout main
    Switched to branch 'main'
    $ echo "content to append" >> merge.txt
    $ git commit -am"appended content to merge.txt"
    $ git push
    ```

### Now since we have some future changes on our `main` branch which overlap with previous changes in our `new_branch_to_merge_later` we will see merge conflicts if we try to merge the `main` branch onto the `new_branch_to_merge_later`.

    ```
    $ git merge new_branch_to_merge_later
    Auto-merging merge.txt
    CONFLICT (content): Merge conflict in merge.txt
    Automatic merge failed; fix conflicts and then commit the result.
    ```

## How to Read the Conflict Files.

### Now that we are in a situation where we have seen a merge conflict we will try to resolve it. First lets see what the merge conflict is. if we do git status we should see the files which have conflicts within them.

    ```
    $ git status
    On branch main
    You have unmerged paths.
    (fix conflicts and run "git commit")
    (use "git merge --abort" to abort the merge)

    Unmerged paths:
    (use "git add <file>..." to mark resolution)

    both modified:   merge.txt
    ```

### When we take a look at the merge conflicts file we will see that there are are the conflicts of the two branches between the `<<<<<<<` and `>>>>>>>` markers within.  The changes on top are the changes which are coming from main, and the changes at the bottom are ones coming from the branch we are trying to merge onto in this case `new_branch_to_merge_later`.

    ```BASH
    $ cat merge.txt
    <<<<<<< HEAD
    this is some content to mess with
    content to append
    =======
    totally different content to merge later
    >>>>>>> new_branch_to_merge_later
    ```

## Fixing Them
### You should open the `merge.txt` file and choose which changes you want to keep, while ofcourse removing the lines with the <<<<<<<,  >>>>>>>, and ======== in this case we will comebine the two lines.

    ```
    this is some content to mess with
    content to append
    totally different content to merge later
    ```

### now that we have resolved this conflict lets commit it this will essentially resolve the conflict for good

    ```
    $ git commit -am "merged and resolved the conflict in merge.txt"
    $ git push --force-with-lease
    ```

# Resolving Diverged Branches

## Creating a diverged branch

### In order to create a diverged branch you need to have some changes that are in your remote branch which aren't in your local branch, this causes you to not be able to push changes to the remote branch. To do this we will create another branch and submit a pr in github into the main branch then try to make changes on the local branch of main which conflict with the remote changes.  This usually only happens if you don't PULL changes before making your own changes to your local branch



####