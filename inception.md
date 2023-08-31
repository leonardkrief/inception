# INCEPTION CHEATSHEET

### SYSTEM COMMANDS

``` bash
apt-get -y update
apt-get -y upgrade
```
check for updates for all libraries, then upgrade them with the new detected updates

``` bash
su -
```
connects as superuser (root). Will ask for password

``` bash
usermod -aG sudo my_username
```
adds my_username to sudo group.
usermod command is commonly used to change a user's login name, home directory, shell, password expiry information, etc.

``` bash
getent group sudo
```
get the entries in the group sudo

``` bash
sudo visudo
```
edit the sudoers file with root privileges in a safe and secure manner because if a mistake is made while editing sudo file without guards, it could prevent you from gaining root access again  
visudo:
- locks the sudoers file against simultaneous ed
- performs a syntax check on sudoers file to avoid syntax errors
- provides a controlled environment using the system's default text editor, typically ensuring that the file is edited in a safe manner  
  
### UFW

``` bash
sudo ufw app list
```
lists all available apps with ufw

``` bash
sudo ufw allow app_name
```
allows app_name app with ufw

``` bash
sudo ufw allow port_number
```
opens the port port_number, allows machines to connect to it

``` bash
sudo ufw status
```
enable the firewall then checks if it is active

``` bash
sudo ufw enable
```


### CONNECT TO YOUR VM THROUGH SSH

Once you setup **ssh** and **ufw** on your vm, you will be able to connect to your vm through any other terminal.

1. open a port on your **guest machine** (your VM). I choose the port 4242 here, but you can choose whatever free port you wish:
    ``` bash
    sudo ufw allow 4242
    ```

2. check that your ssh service is active:
    ``` bash
    systemctl status ssh
    ```

    If not, turn it on:
    ```bash
    sudo systemctl start ssh
    ```

3. go on your VM's hostpage on VirtualBox and get in the following section:  
    ```Settings -> Network -> Advanced -> Port Forwarding ```  
Create a new entry with ```Host Port = 4243``` and ```Guest Port = 4242```.  
Basically, the host port is the port you wish to connect through on your local machine (the terminal you are about to connect from) and the guest port is the port you will connect to through your VM (here 4242).

4. open a new terminal on your host machine and type in the following command:
    ```bash
    ssh localhost@your_username -p 4243
    ```  
    where your_username should be your username on your VM.  
    You will then be asked to enter your password and there you are !

### SETUP DOCKER

Follow the instructions through this link: https://docs.docker.com/engine/install/debian/#install-using-the-repository

Then add your name to the docker group:
```bash
sudo usermod -aG docker ${USER}
```
To apply the new group membership, log out of the server and back in or type the following (you should enter your username password, not the root one):
```bash
su - ${USER}
```

To confirm it worked, the following command should display ```docker```:
```bash
id -nG | grep -o 'docker'
```

### INITIALIZE YOUR FILES AS THE SUBJECT REQUESTS

Go to the folder that will contain your project and pqste in:
```bash
touch Makefile
mkdir -p srcs/requirements/{bonus,mariadb/{conf,tools},nginx/{conf,tools},wordpress/{conf,tools},tools}
touch srcs/docker-compose.yml
echo -e "DOMAIN_NAME=xxx.42.fr\n# certificates\nCERTS_=./xxx\n# MYSQL SETUP\nMYSQL_ROOT_PASSWORD=xxx\nMYSQL_USER=xxx\nMYSQL_PASSWORD=xxx" > srcs/.env
touch srcs/requirements/mariadb/{Dockerfile,.dockerignore,conf/mariadb.conf}
touch srcs/requirements/nginx/{Dockerfile,.dockerignore,conf/nginx.conf}
ls -alR
```


