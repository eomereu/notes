## Delete A Commit Permenantly and Properly
1. We can use `rebase` to delete commits,
   ```bash
   $ git rebase -i HEAD~10
   ```
   For example the code above will open us our favorite editor with listing the last 10 commits we made. But here it lists from oldest to newest from top to bottom,
    ```
     pick 5c6eb73 Added repo.or.cz link
     pick a311a64 Reordered analogies in "Work How You Want"
     pick 100834f Added push target to Makefile
    ```
   So here, 5c6eb73 is the oldest commit, and 100834f is the newest.  Then we can,
     - Remove commits by deleting lines. Like the revert command, but off the record: it will be as if the commit never existed.
     - Reorder commits by reordering lines.
     - Replace pick with:
       - `edit` to mark a commit for amending.
       - `reword` to change the log message.
       - `squash` to merge a commit with the previous one.
       - `fixup` to merge a commit with the previous one and discard the log message.
     [More on](http://www-cs-students.stanford.edu/~blynn/gitmagic/ch05.html#_8230_and_then_some)
   [Lecture on rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)

2. After deleting the commits, we have to push our updated local commits to our online git repository,
   ```
   $ git push --force
   ```

***

## Pushing to GitHub via SSH Key
This allows us to push things without entering id password every time:
1. First of all we have to [generate an SSH key](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) on our machine,
   ```bash
   $ ssh-keygen -t rsa -b 4096 -C "our_email@example.com"
   ```
  
2. This creates a new ssh key, using the provided email as a label.
   ```
   > Generating public/private rsa key pair.
   ```

3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.
   ```
   > Enter a file in which to save the key (/home/us/.ssh/id_rsa): [Press enter]
   ```

4. At the prompt, type a secure passphrase.
   ```
   > Enter passphrase (empty for no passphrase): [Type a passphrase]
   > Enter same passphrase again: [Type passphrase again]
   ```
   - [To change the passphrase](https://docs.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases),
     ```
     $ ssh-keygen -p
     # Start the SSH key creation process
     > Enter file in which the key is (/Users/you/.ssh/id_rsa): [Hit enter]
     > Key has comment '/Users/you/.ssh/id_rsa'
     > Enter new passphrase (empty for no passphrase): [Type new passphrase]
     > Enter same passphrase again: [One more time for luck]
     > Your identification has been saved with the new passphrase.
     ```

5. After creating SSH key successfully let's add it to our GitHub account, under *Settings* click on *SSH and GPG keys* tab and hit **New SSH Key** button. Paste in the key on the opened window. To get this key,
   ```bash
   $ vi ~/.ssh/id_rsa.pub
   ```
   Copy exactly from the end (with be the last letter of your email domain extension) of the block of characters to the beginning, which starts with ssh-rs ...

6. After completing till here now let's update our remote git links to SSH ones if they're already in HTTPS. To do this go to the related folder and type,
   ```
   $ git remote set-url origin git@github.com:<Username>/<Project>.git
   ```
   Otherwise it will keep asking id password since it is binded to HTTPS connection!


***

## How to Clone All Remote Branches
Right after simply cloning and just getting only our 'master' branch of a git repository, we can simply [check all our remote branches](https://stackoverflow.com/questions/67699/how-to-clone-all-remote-branches-in-git) in the directory where we cloned the repository into with,
```bash
$ git branch -a
```
After this we will see i.e.
```
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/test
  remotes/origin/someotherbranch
```
At this point we can take a quick peek at an upstream branch,
```bash
$ git checkout origin/experimental
```
But if we want to work on that branch we need to create a local tracking branch which is done automatically by,
```bash
$ git checkout test
```
and we will see,
```
Branch experimental set up to track remote branch experimental from origin.
Switched to a new branch 'experimental'
```
This actually means that it also got our wanted branch locally and we can work on it from that time on. So if we check local branches, we will see,
```bash
$ git branch
* test
  master
```

***

## Cloning a branch besides 'master' branch
To clone another branch other than master we can type,
```bash
$ git clone -b mybranch --single-branch git://sub.domain.com/repo.git
```

***

- [*Nano Editor* cheat sheet](https://monovm.com/post/35/how-to-exit-in-nano)