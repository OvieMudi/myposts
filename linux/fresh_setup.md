This post documents the process I follow to set up my work PC when I acquire a new one or install a new distro (found myself doing this a lot lately).

**Who this post is for:** You're new to the Linux environment, but not a total noob. (i.e you've seen the bash terminal before and have typed `cd, ls, mkdir, touch` e.t.c)

## Table Of Contents
         [Update/Upgrade your packages](#update-packages)
         [Install Chrome Browser](#Install-Chrome-Browser)
         [Install git](#Install-git)
         [Install Node.js](#Install-Node)
         [Install Snap Store](#Install-snap)
         [Install PostgreSQL](#Install-postgres)
         [Install Python](#Install-python)
         [The End?](#End)


[//]: # (================================================================)

### Update/Upgrade your Packages  <a name="update-packages"></a>
Here are two commands that you should run immediately after installing your favorite Linux distro(Debian-based). Not running these commands at first boot makes you a rebel😐️.
Running the commands below ensures that all your packages are updated, including drivers needed for your system to function properly.

```bash
# Update Linux package lists
sudo apt update

# Upgrade packages
sudo apt upgrade
```
`apt update` simply tells Linux package manager to find updates for configured package sources from the internet. This ensures you have the latest links from which you can upgrade your packages.

`apt upgrade` downloads the updated packages from the sources, and installs them. It upgrades existing packages.

`sudo` (Super-user do) simply means you must have super-user (admin) rights to perform the following operation. 

[//]: # (================================================================)

### Install Chrome Browser <a name="Install-Chrome-Browser"></a>

```bash
# Download the latest chrome package
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

# Install the Chrome package
sudo dpkg -i google-chrome-stable_current_amd64.deb

```
Confirm that the Chrome repo is added to your sources list by running the following command (This ensures updates are downloaded and installed automatically).

` ls /etc/apt/sources.list.d/ | grep 'chrome' `  👇️

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/lvtfm8dhbjzyqbuh2kkf.png)
You should find `google-chrome.list` in the result as shown above. 👆️


[//]: # (================================================================)

### Install git <a name="Install-git"></a>

   - Install git from apt

```bash
# Update packages
sudo apt update

# Install git
sudo apt install git

# Confirm that git is installed
git --version
```
   - Configure git
```bash
# Configure default name and email
git config --global user. name "Your Name"
git config --global user.email "your@email.com"

# View the configuration file
cat ~/.gitconfig 
```


[//]: # (================================================================)

### Install Node.js <a name="Install-Node"></a>

We'll install Node via NVM. NVM is a version manager for Node. It basically allows you to easily install and switch between different Node versions.

- **Run the NVM install script**.

At the time of writing this, the latest version was ` v0.35.1 `. Check the [NVM repo](https://github.com/nvm-sh/nvm/blob/master/README.md) for the latest version.


```bash
# Fetch/install NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash

# Include NVM in the current terminal session
source ~/.profile

# Confirm that NVM is active
nvm --version

```

- **Install Node with NVM**

  - We'll install the latest LTS version available (v12.13.0 at the time of writing).

  ```bash
# Install the latest LTS version
nvm install --lts

# Confirm that Node/NPM  installed
node --version
npm --version
```
To install other Node versions, run ` nvm ls-remote ` to fetch all available Node versions.
Install your preferred version by running ` nvm install <node_version> `.
For example ` nvm install 10 `. See [The docs](https://github.com/nvm-sh/nvm/blob/master/README.md#usage) for more examples.



[//]: # (================================================================)

### Install Snap Store <a name="Install-snap"></a>

Snap is a software package management system, or simply, the AppStore for Linux.
_Note that if you're using Ubuntu 16.04 LTS or higher, kindly skip this section as Snap is already installed out of the box._

```bash
# Update packages
sudo apt update

# Install Snap
sudo apt install snapd

# Install Snap Store
sudo snap install snap-store
```
You can now launch Snap Store from your applications menu and start installing your favorite applications with the awesome GUI. 👇️

![Snap Store GUI](https://thepracticaldev.s3.amazonaws.com/i/nwb851w9c5n0575xr0sw.png)

_If you can't find the Snap Store icon from your menu, you may need to restart your computer._



[//]: # (================================================================)

### Install PostgreSQL <a name="Install-postgres"></a>
- Install Postgres from apt

```bash
# Update packages
sudo apt update

# Install PostgreSQL
sudo apt install postgresql postgresql-contrib



# Start psql shell
sudo -u postgres psql
```
[//]: # (==== Todo: add screenshot of psql shell =====)

- Create a new Postgres role (user) from psql

```sql
-- Create a new role
CREATE ROLE role_name WITH PASSWORD 'role_password';

-- Grant access to the new role
ALTER ROLE role_name LOGIN CREATEDB CREATEROLE;

-- Create a database
CREATE DATABASE db_name;
```
- Run ` \q ` to quit psql shell.
- Run ` psql -U role_name -d db_name ` to log in with the created role.
- Run ` \conninfo ` from the shell to get details of your Postgres connection.

If you prefer GUI to psql, install your favorite DB management tool.
👉️ **PGAdmin 3**:  ` sudo apt-get install pgadmin3`
👉️ **DBeaver Community Edition**: Search and install from [Snap Store](#Install-snap),
 or run ` sudo snap install dbeaver-ce --edge `



[//]: # (================================================================)

### Install Python <a name="Install-python"></a>

- Install Python 3.7

```bash
# Install/upgrade software-properties-common
sudo apt update
sudo apt install software-properties-common

# Add the deadsnakes PPA for Python
sudo add-apt-repository ppa:deadsnakes/ppa

# Install Python 3.7
sudo apt update
sudo apt install python3.7

# View python3.7 version
python3.7 --version
```
- Install pip for Python 3.7

```bash
# Install pip from apt
sudo apt install python3-pip

# Install pip for Python 3.7
python3.7 -m pip install pip
```
pip3 is "linked" to the system's default Python 3.6. We need to create an alias that links pip3 to python3.7.
_Note that there are other methods for achieving this, but I believe aliases are safest for us beginners._ 😁️

- Add a new alias to your .bash_aliases

```bash
# Create/update .bash_aliases with pip3 alias
echo "alias pip3='python3.7 -m pip'" >> ~/.bash_aliases

# Restart bash session to reflect changes
exec bash
```

- Install Python virtual environment

```bash
# Install virtualenv
pip3 install virtualenv --user
```

- Install dependencies for psycopg
  
  You may encounter errors while trying to use psycopg in a Python virtual environment. Installing these dependencies will most likely fix the issue:

```bash
# Update packages
sudo apt-get update

# Install libpq-dev
sudo apt-get install libpq-dev 

# If errors persist then we gear up 👇️

# Install python3.7-dev
sudo apt-get install libpython3.7-minimal=3.7.3-2~18.04.1
sudo apt-get install python3.7-dev
```

[//]: # (================================================================)


### The End? <a name="End"></a>

We have **NOT** yet come to the end of this post!
![This-is-not-complete](https://thepracticaldev.s3.amazonaws.com/i/k9b4mfb8suqv0xeqjuq6.gif)
Yeah. I'll be updating this post based on your recommendations so that more beginners can have a good first experience setting up their Linux environment for development (Web/Mobile).

Please feel free to drop your questions, suggestions, and critic in the comments. Thank you very much!
