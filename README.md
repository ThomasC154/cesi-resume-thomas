# Web Starter Kit
## Install Ubuntu 18.04 Desktop
1. Download
ubuntu-18.04.3-desktop-amd64.iso
2. Create a new VirtualMachine in VMware Workstation or in Oracle VirtualBox
    * Memory: 4Go
    * Hard Disk: 20Go
    * Network: NAT (or Bridge)

## Install VMware tools
Open a **Terminal** and type:

    $ sudo apt upgrade
    $ sudo apt install open-vm-tools-desktop

## Install VirtualBox Guest additions tools
In the opened **Terminal** type:

    $ sudo apt install build-essential dkms linux-headers-$(uname -r) 

In VirtualBox menu click **Devices/Insert Guest Additions CD image**. If an error occurs because the image was already inserted, then type:

    $ sudo rcvboxadd setup

Reboot the virtual machine.

From the **Machine/Settings** menu, in the **Advanced** panel, choose **Bidirectional Shared Clipboard** and **Bidirectional Drag'n'Drop**. Add also the Code directory as a shared folder by clicking **Shared Folders** then the plus sign.

In the virtual machine change the language to English.

## Add SSH keys to GitHub
### Generate
Open a **Terminal** and type:

    $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

Press Enter to accept the default file location and enter a passphrase you'll remember.

    $ eval "$(ssh-agent -s)"
    $ ssh-add ~/.ssh/id_rsa

Detailed instructions: https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/

### Add to GitHub

    $ cat ~/.ssh/id_rsa.pub

Copy the content of this file to your clipboard and paste it to GitHub **Settings / SSH and GPG keys / New SSH key**. No need to install xclip.

Detailed instruction: https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/

### Turn on Git credential helper (if cloning repository using HTTPS)

    $ git config --global credential.helper cache
    $ git config --global credential.helper 'cache --timeout=3600'

Detailed instruction: https://help.github.com/articles/caching-your-github-password-in-git/

## Install Docker CE
Please refer to the detailed instructions below while typing the following commands.

    $ sudo apt install \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    $ sudo add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"
    $ sudo apt update
    $ sudo apt install docker-ce docker-ce-cli containerd.io
    $ docker -v
    $ sudo docker run hello-world

Detailed instructions: 
https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository

Post-installation

    $ sudo groupadd docker
    $ sudo usermod -aG docker $USER
    $ newgrp docker
    $ docker run hello-world

Detailed instructions:
https://docs.docker.com/install/linux/linux-postinstall/

## Install Docker Compose
Please refer to the detailed instructions below while typing the following commands.

    $ sudo curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
    $ sudo chmod +x /usr/local/bin/docker-compose
    $ docker-compose -v

Detailed instructions: https://docs.docker.com/compose/install/#install-compose

## Install Visual Studio Code
Download .deb file at https://code.visualstudio.com/docs/?dv=linux64_deb

Please refer to the detailed instructions below while typing the following commands and adjust them to the file you downloaded.

    $ cd Downloads/
    $ sudo dpkg -i code_*_amd64.deb
    $ sudo apt install -f
    $ code

Detailed instructions: https://code.visualstudio.com/docs/setup/linux

## Create working directory Code
Please ensure you write all your code in the ~/Code folder. Breath deep and take a snapshot.

    $ mkdir ~/Code
    $ cd ~/Code
    $ git clone this_repository.git # this_repository.git refers to your actual GitHub repository
    $ cd this_repository
    $ docker-compose up -d # if you encounter an error at this step, restart Ubuntu and try again

Visit http://localhost:8080

Open this project with Visual Studio Code, change the text in the h1 tag of index.html and reload the page to see the changes. Does it work ?

## Install Sass
    $ sudo apt install ruby-full
    $ sudo gem install sass --no-user-install

Detailed instructions:
* https://www.ruby-lang.org/fr/documentation/installation/#apt
* https://sass-lang.com/install

## Ready to code
Following commands require to be in your project directory ~/Code/this_repository.

    $ sass --watch sass:css

You are now free to code, but don't modify the content of the css directory. Every modification of scss/app.scss file will update files located in the css directory.

## Deploy

Sign up for a free Heroku account: https://www.heroku.com/ 

Follow these steps:
* Create a new app
    * App name: cesi-resume-yourforname (for example cesi-resume-eric)
    * Choose a region: Europe
* Connect to Github
    * Deployment method: Github
    * Connect to Github: enter your repository name and click "search" and "connect"
* Deploy
    * Manual deploy: click "Deploy branch"

Visit https://cesi-resume-yourforname.herokuapp.com

## Git
### Configuration

    git config --global user.email ID+username@users.noreply.github.com
    git commit --amend --reset-author # to update last commit (only if you did one of course)

Finding your commit email address: https://help.github.com/articles/about-commit-email-addresses/

Detailed instructions: https://help.github.com/articles/setting-your-commit-email-address-in-git/

### Commands
Useful git commands:

    git status
    git diff
    git log
    git add filename
    git commit -m "message"
    git commit --amend
    git pull origin master
    git push origin master

Detailled instructions: https://www.git-tower.com/blog/git-cheat-sheet/

### Commit
Commit message format "type: subject"
* type: one of the following ones
    * feat (new feature)
    * fix (bug fix)
    * docs (changes to documentation)
    * style (formatting, missing semi colons, etc; no code change)
    * refactor (refactoring production code)
    * test (adding missing tests, refactoring tests; no production code change)
    * chore (updating grunt tasks etc; no production code change)
* subject: summary in present tense

Examples: https://seesparkbox.com/foundry/semantic_commit_messages 

Detailled instructions: https://conventionalcommits.org/

#### Commit example
    git add docker-compose.yml
    git commit -m "chore: add docker-compose.yml file" -m "Create three services (mariadb, laravel and redis) and one volume (db_data). Type \"docker-compose up\" to start"
    git push origin master 

#### Initial commit example for a new project
    git init
    git add .
    git remote add origin https://github.com/username/repository.git
    git commit -m "chore: add initial code"
    git push -u origin master 

### Tag

Detailled instructions: https://semver.org/

#### Tag example
    git tag -a 0.1.0 -m "Initial development version 0.1.0"
    git push origin master --tags
