# How-to-set-up-Multiple-Git-accounts-in-Bash
Here i describe, how to add multiple accounts to Git Bash to each when needed

**Open git bash** in `~/.ssh/` directory 

Create two ssh keys
```bash
ssh-keygen -t rsa -b 4096 -C "email1@example.com"
```
and second one 
```bash
ssh-keygen -t rsa -b 4096 -C "email2@example.com"
```

**When prompted**:
```
Enter file in which to save the key (/c/Users/User/.ssh/id_rsa):
```
you can skip that pressing `Enter` and use default location 

or insert your own file name `(by default it's id_rsa)`

Alternatively you may specify another location by typing `/d/git/ssh-keys/filename` ==(Not Recommended)==

Add keys to SSH agent
```bash
#open ssh agent
eval "$(ssh-agent -s)"
#add first key
ssh-add ~/.ssh/id_rsa #or another path
#add second key
ssh-add ~/.ssh/id_rsa_account2
```

Next configure config file in `~/.ssh` directory
Create file, if it isn't there
```bash
touch config #without extension
```
Open the file in any text editor or IDE and add the following configuration:

In field `Host` enter an alias for the account (use something memorable; you'll use it later).

In `HostName` enter `github.com` 

`User` field should contain `git`

In `IdentutyFile` specify path to your ssh private(without extesion `.pub`) key.

You may or not specify IdentityOnly field, it is not necessarily.

Then repeat the same for another account

```text
Host job-account 
HostName github.com
User git
IdentityFile ~/.ssh/job
IdentitiesOnly yes

Host personal-account
HostName github.com
User git
IdentityFile ~/.ssh/personal
IdentitiesOnly yes
```

Open `github.com`, account settings and folder `SSH and GPG keys` add new SSH key. 

Copy your generated public (.pub extension) key and insert it in the ssh field

Same for another account

Now to `clone` git repos you should use SSH
![[Pasted image 20250113131802.png]]

copy ssh key and insert it into git clone command so it would look like this:
```
git clone git@yourAlliesName:ownerName/example-repo.git
```

make sure to insert after `@` your Allies name from config file. next goes `ownerName` and then the repository name.

Run the following command to ensure everything is set up correctly
```bash
ssh -T git@alliesName
```
If successful, you'll see a message like:        
```text
Hi username! You've successfully authenticated, but GitHub does not provide shell access. 
```
