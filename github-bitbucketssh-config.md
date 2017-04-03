# Setting up github and bitbucket on the same computer
Github will be the main account and bitbucket the secondary.

## Create SSH Keys  

`ssh-keygen -t rsa -C "github email"`

Enter passphrase when prompted. If you see an option to save the passphrase in
your keychain, **do it** for an easier life.

Save keys to:  

`~/.ssh/id_rsa`  

Repeat for bitbucket:

`ssh-keygen -t rsa -C "bitbucket email"`

Save bitbucket key to `~/.ssh/id_rsa_bb`  

## Attach Keys  
Login to remote repo and add ssh key:

```shell
pbcopy < ~/.ssh/id_rsa.pub
pbcopy < ~/.ssh/id_rsa_bb.pub
```

Obviously run each `pbcopy` command **individually.**

Paste into text area, under ssh settings, in your github or bitbucket account.
Also give the ssh key a title like "your name" Laptop.  

## Create Config file  
I am using vim, enter your editor here if different:

`vim ~/.ssh/config`

Create your git aliases like so:

```vim
#Github (default)
  Host gh
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

#Bitbucket (secondary)
  Host bb
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_rsa_bb
```  

## Add the identities to SSH:  

```shell
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_bb
```

Enter passphrase if prompted.

Check keys were added:

`ssh-add -l`  

## Check that repo recognizes keys:  

```shell
ssh -T gh
ssh -T bb
```  


################################################################################
# Using new dual setup
################################################################################

## Using new dual setup
*This guide assumes you have followed `multi-git.md` and set up both accounts.*

### Github (default)
Create a repo online called testmulti (or one of your choosing), then in Terminal,
create your repository:

```shell
mkdir ~/testmulti
cd ~/testmulti
touch readme.md
git init
git add .
git commit -am "first commit"
git remote add origin git@gh:username/testmulti.git
git push origin master
```

Add some text to readme on github.com, then:

```shell
git fetch origin master
git diff master origin/master
git merge origin/master
git push origin master
```

### Bitbucket (secondary)
Create a repo online called testbucket and then in Terminal:

```shell
mkdir ~/testbucket
cd ~/testbucket
touch readme.md
git init
```

*This being the secondary account, the username and email have to be
overwritten, using secondary account values, at the repo level:*

```shell
git config user.name "Full Name"
git config user.email email_address
```

This must be done **once** for every *bitbucket* (or secondary) repo, it is not 
needed for github (or primary) repos because **the global is used** in that scenario. 
There may be a cleaner way to do this but right now it works okay.  

Just to be clear you do not need to change these values back afterwards because the 
global values (which apply to all future repos created) will be set.  

```shell
git add .
git commit -am "first commit"
git remote add origin git@bb:username/testbucket.git
git push origin master
```  

Add some text to readme on bitbucket.org, then:  

```shell
git fetch origin master
git diff master origin/master
git merge origin/master
git push origin master
```

## Use case: Repo already exists on secondary remote repo (bitbucket)
So you have a repository that already exists and you want to want to `clone` it 
but also you want to make sure when you `push` it, the correct user, the bitbucket 
user pushes. Let's say the repo is called `booker`.

```shell
git clone git@bb:bb_username/booker.git
git config user.name "bitbucket user name"
git config user.email email_address
```

Remember, because we are using the secondary account, we have to over-ride the global configuration of username and email. The git clone command is using the ssh keyfile you set up earlier and is doing the transfer over ssh.

Now, change into the cloned directory and modify one of the files. Use `git status` to check the current state, then use `git add 'filename'` and `git commit -m "commit message"`.

Finally push your changes using `git push origin master`.



################################################################################
#
################################################################################


## Using new dual setup
*This guide assumes you have followed `multi-git.md` and set up both accounts.*

### Github (default)
Create a repo online called testmulti (or one of your choosing), then in Terminal,
create your repository:

```shell
mkdir ~/testmulti
cd ~/testmulti
touch readme.md
git init
git add .
git commit -am "first commit"
git remote add origin git@gh:username/testmulti.git
git push origin master
```

Add some text to readme on github.com, then:

```shell
git fetch origin master
git diff master origin/master
git merge origin/master
git push origin master
```

### Bitbucket (secondary)
Create a repo online called testbucket and then in Terminal:

```shell
mkdir ~/testbucket
cd ~/testbucket
touch readme.md
git init
```

*This being the secondary account, the username and email have to be
overwritten, using secondary account values, at the repo level:*

```shell
git config user.name "Full Name"
git config user.email email_address
```

This must be done **once** for every *bitbucket* (or secondary) repo, it is not 
needed for github (or primary) repos because **the global is used** in that scenario. 
There may be a cleaner way to do this but right now it works okay.  

Just to be clear you do not need to change these values back afterwards because the 
global values (which apply to all future repos created) will be set.  

```shell
git add .
git commit -am "first commit"
git remote add origin git@bb:username/testbucket.git
git push origin master
```  

Add some text to readme on bitbucket.org, then:  

```shell
git fetch origin master
git diff master origin/master
git merge origin/master
git push origin master
```

## Use case: Repo already exists on secondary remote repo (bitbucket)
So you have a repository that already exists and you want to want to `clone` it 
but also you want to make sure when you `push` it, the correct user, the bitbucket 
user pushes. Let's say the repo is called `booker`.

```shell
git clone git@bb:bb_username/booker.git
git config user.name "bitbucket user name"
git config user.email email_address
```

Remember, because we are using the secondary account, we have to over-ride the global configuration of username and email. The git clone command is using the ssh keyfile you set up earlier and is doing the transfer over ssh.

Now, change into the cloned directory and modify one of the files. Use `git status` to check the current state, then use `git add 'filename'` and `git commit -m "commit message"`.

Finally push your changes using `git push origin master`.
