# Multiple Git account management

### 0. Prepare
Check folder ~/.ssh exist?
If not:
```
mkdir ~/.ssh
chmod 700 ~/.ssh
```
Move into .ssh dir
```
cd ~/.ssh
```

### 1. Generating SSH key
Check to see if we have any existing SSH keys `ls -al ~/.ssh`\
If `~/.ssh/id_rsa` not exist, generate:
```
ssh-keygen -t rsa
```
For different account (work, freelance,...), we create different SSH keys.\
Public key will be saved with the tag `email@work_mail.com` to `~/.ssh/id_rsa_work_user.pub`\
Note: work_user should be the same with user in git ssh url
`git@gitlab.com:seedcom/rails-sample-api.git`\
-> work_user = seedcom
```
ssh-keygen -t rsa -C "email@work_mail.com" -f "id_rsa_work_user"
```

### 2. Registering new SSH keys with ssh-agent
To use the keys, we have to register them with the ssh-agent

Start ssh-agent
```
eval "$(ssh-agent -s)"
```
Add the keys to the ssh-agent
```
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_work_user
```

### 3. SSH config file
```
cd ~/.ssh/
touch config		// Creates the file if not exists
```
Edit file `~/.ssh/config` with editor
```
# Personal account, - the default config
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

# Work account-1
Host github.com-work_user		// any name you want
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_work_user
```

### 4. Adding the new SSH key to the GitHub account
1. Copy public key `pbcopy < ~/.ssh/id_rsa.pub`
2. Go to github `Setting`
3. Select `SSH and GPG keys` from the menu
4. Click on `New SSH key`, add paste the key in the box
5. Click `Add key` - DONE
> For the work accounts: `pbcopy < ~/.ssh/id_rsa_work.pub`

## Note: clone repo with SSH
Default
```
git clone git@github.com:personal_account_name/repo_name.git
```
Work account
```
git clone git@github.com-work_user:work_user/repo_name.git
```
My clone with SSH from gitlab
`git@gitlab.com:seedcom/rails-sample-api.git`\
```
git clone git@gitlab.com-work_user:seedcom/repo_name.git
```

Source: https://www.freecodecamp.org/news/manage-multiple-github-accounts-the-ssh-way-2dadc30ccaca/
