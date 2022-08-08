# Cuckoo installtion giude for Spamurai

![cuckoo](https://img.shields.io/badge/opensource-Cuckoo--Sandbox-brightgreen)
![malware](https://img.shields.io/badge/analysis-malware-brightgreen)
![attachment](https://img.shields.io/badge/email-attachment-brightgreen)
![project](https://img.shields.io/badge/project-Spamurai-blue)
![Offline](https://img.shields.io/badge/Offline-Sandbox-red)


![Spamurai-cuckoo](/assets/attach/logos3.png)


## introduction
Cuckoo is an open source automated malware analysis system.

It’s used to automatically run and analyze files and collect comprehensive analysis results that outline what the malware does while running inside an isolated operating system.

It can retrieve the following type of results:

Traces of calls performed by all processes spawned by the malware.
Files being created, deleted and downloaded by the malware during its execution.
Memory dumps of the malware processes.
Network traffic trace in PCAP format.
Screenshots taken during the execution of the malware.
Full memory dumps of the machines.

It can be used to analyze:

Generic Windows executables
DLL files
PDF documents
Microsoft Office documents
URLs and HTML files
PHP scripts
CPL files
Visual Basic (VB) scripts
ZIP files
Java JAR
Python files
Almost anything else

[Cuckoo documentation](https://cuckoo.sh/docs/introduction/what.html)

Before start we need to understand how it works
First of all , Cuckoo runs on host machine (ubuntu 18.04) and with virtuallization we create guests (like win7,xp,,,etc) depends on the malware you want to analysis and it's requirments.
you create your guest virtual machine with the needed requriements of malware 
example if malware is injected on .pdf file , it will need Adobe acrobat reader to open pdf file. 

Our documentation and installation process can be more clarified by this [tutorial](https://www.youtube.com/watch?v=fbt4fk5qiow&t=914s&ab_channel=OpenSecure), Thanks for [OpenSource](https://www.youtube.com/channel/UC4EUQtTxeC8wGrKRafI6pZg) channel.

## Requirments
1- Vmware workstation / Virtualbox (any virtualization software )
2- ubuntu 18.04 LTS
3- Win7UltimateX64.iso (my guest)
4- any other guests you need to build.


## Preparing THE Machine

I am gonna go with VMware® Workstation 16 Pro.

Download and Install it through [vmware website](https://www.vmware.com/mena/products/workstation-pro/workstation-pro-evaluation.html)

Download your host operating system in my case is Ubuntu 18.04.6 LTS (Bionic Beaver) downloadable [here](https://releases.ubuntu.com/18.04/) 


Create your vm machine which will be your host machine later on.
From file menu in VMware workstation ,select New Virtual machine go through the steps , choose your ubuntu iso file and the needed info.
It's pretty easy and simple.
Run your machine up and start the real work.


## Preparing THE HOST

As an linux expert user do your steps to setup your enviroment your updates and all that kind of stuff. and start setting up your host.
```
$ sudo apt-get update
$ sudo apt upgrade
```
#### Python 

The Cuckoo host components is completely written in Python 2.7 so we need to configure some needed dependancies.

#### Database Software

In order to use PostgreSQL as database (our recommendation), PostgreSQL will have to be installed as well.

#### Virtualization Software

Cuckoo Sandbox supports most Virtualization Software solutions.
I just want to clarify this point , i know we had been already used VMware as virtualization software but this is to build our main machine which is the host (ubuntu machine) but regarding this point we need to create more machines inside ubuntu machine as we will call them (guests)
As cuckoo need to connect with these guests to use them as a sandbox machines to can analysis malwares without affecting our host or our personal machines. to be more secure and isolated.

Indeed in real talk , our project deal with mail server which is mostly linux based os, so be ready to deal with it from now. 

but i will go to virtualbox regarding virtualization through ubuntu you can choose whatever you want.

#### Installing tcpdump
In order to dump the network activity performed by the malware during execution, you’ll need a network sniffer properly configured to capture the traffic and dump it to a file.

By default Cuckoo adopts tcpdump, the prominent open source solution.

The following software packages from the apt repositories are required to get Cuckoo to install and run properly for all previous clarified and needed softwares:

```
$ sudo apt-get install python python-pip python-dev libffi-dev libssl-dev -y
$ sudo apt-get install python-virtualenv python-setuptools -y
$ sudo apt-get install libjpeg-dev zlib1g-dev swig -y
$ sudo apt-get install postgresql libpq-dev -y
$ sudo apt install virtualbox -y
$ sudo apt-get install tcpdump apparmor-utils -y
```

In order to use the Django-based Web Interface, MongoDB is required:
And also chech if it running propably.

```
$ sudo apt-get install mongodb -y
$ systemctl status mongodb.service
```
Mongodb should give you Active(running) status

Tell now , pretty easy and simple , hope no annoying errors to keep focus on the upcoming steps.
### Adding "Cuckoo" User (optional)
**Incase using another user and want to add cuckoo user**
```
sudo adduser --disabled-password --gecos "" cuckoo
```
#### Adding group and previlages to group and cuckoo user to can manage cuckoo probably

```
$ sudo groupadd pcap
$ sudo usermod -a -G pcap cuckoo
$ sudo chgrp pcap /usr/sbin/tcpdump
$ sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump
```
You can verify the results of the last command with:

```
$ getcap /usr/sbin/tcpdump
/usr/sbin/tcpdump = cap_net_admin,cap_net_raw+eip
```
Last command to disable the Apparmour profile for tcpdumb

Note that the AppArmor profile disabling (the aa-disable command) is only required when using the default CWD directory as AppArmor would otherwise prevent the creation of the actual PCAP files

```
$ sudo aa-disable /usr/sbin/tcpdump
```

#### Installing M2Crypto
 
 Currently the M2Crypto library is only supported when SWIG has been installed. On Ubuntu/Debian-like systems this may be done as follows:

Alread installed before , incase missed :
```
$ sudo apt-get install swig
````

If SWIG is present on the system one may install M2Crypto as follows:

```
$ sudo pip install m2crypto
```
Give the ability to our user (cuckoo) to run Vbox and switch user to it
```
$ sudo usermod -a -G vboxusers cuckoo
$ sudo su cuckoo
```

#### Create Venv wrapper
Create our virtual environment wrapper so we can install all dependancies needed.
we will copy and paste a script (cuckoo_setup_venv.sh)which automate the venv creation as follows:
```
$ cd /opt/
/opt$ sudo nano cuckoo_setup_venv.sh
(Copy and paste the following inside that .sh file)
-----------------------------------------
#!/usr/bin/env bash

# NOTES: Run this script as: sudo -u <USERNAME> cuckoo-setup-virtualenv.sh

# install virtualenv
sudo apt-get update && sudo apt-get -y install virtualenv

# install virtualenvwrapper
sudo apt-get -y install virtualenvwrapper

echo "source /usr/share/virtualenvwrapper/virtualenvwrapper.sh" >> ~/.bashrc

# install pip for python3
sudo apt-get -y install python3-pip

# turn on bash auto-complete for pip
pip3 completion --bash >> ~/.bashrc

# avoid installing with root
pip3 install --user virtualenvwrapper

echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3" >> ~/.bashrc

echo "source ~/.local/bin/virtualenvwrapper.sh" >> ~/.bashrc

export WORKON_HOME=~/.virtualenvs

echo "export WORKON_HOME=~/.virtualenvs" >> ~/.bashrc

echo "export PIP_VIRTUALENV_BASE=~/.virtualenvs" >> ~/.bashrc 

---------------------------------
== make it executable to cuckoo user ==
/opt$ sudo chmod +x cuckoo_setup_venv.sh
/opt$ sudo -u cuckoo ./cuckoo_setup_venv.sh
/opt$ source ~/.bashrc
```
#### Creat venv (cuckoo-test)
```
/opt$ mkvirtualenv -p python2.7 cuckoo-test 
(cuckoo-test)/opt$ 
```
Incase you lost your venv path just type `workon <venv_name>`
### Cuckoo Installtion 
Finally will go a head for installing Cuckoo 
here we gonna start install all needed software inside our venv
and then install cuckoo.
```
(cuckoo-test)/opt$ pip install -U pip setuptools
(cuckoo-test)/opt$ pip install -U cuckoo

```

## Preparing THE Guest

Regarding preparing our guests this process can be repeated differently depending the guest.
each guest can has a different requirments but you should be able to identify it as your needs.
Common steps is related to the virtual machine software installation.

### Setup virtual machine
```
(cuckoo-test)/opt$ sudo wget https://cuckoo.sh/win7ultimate.iso
(cuckoo-test)/opt$ sudo mkdir /mnt/win7
(cuckoo-test)/opt$ sudo chown cuckoo:cuckoo /mnt/win7/
```
next step only recommended to be done by root user
```
(cuckoo-test)/opt$ sudo su
root@ubunutu$ sudo mount -o ro,loop win7ultimate.iso /mnt/win7
```
Checking some dependancies before we go
```
(cuckoo-test)/opt$ sudo apt-get -y install build-essential libssl-dev libffi-dev python-dev genisoimage
(cuckoo-test)/opt$ sudo apt-get -y install zlib1g-dev libjpeg-dev
(cuckoo-test)/opt$ sudo apt-get -y install python-pip python-virtualenv (cuckoo-test)/opt$ python-setuptools swig
```
Installing Vmcloak and creating our network host manager (vboxnet0)
Also create our vbox machine (guest machine) with following specs (can be edited depend on your hardware specs) like cpus and ramsize ( i will go for 2 cpus and 2GB ram).
Clone the win7 machine so we can snapshot it.
Just don't panic when you wait a lot , because the following commands gonna take a really lot of time, just be patient.

```
(cuckoo-test)/opt$ pip install -U vmcloak
(cuckoo-test)/opt$ vmcloak-vboxnet0
(cuckoo-test)/opt$vmcloak init --verbose --win7x64 win7x64base --cpus 2 --ramsize 2048
(cuckoo-test)/opt$ vmcloak clone win7x64base win7x64cuckoo
```
here we can list all the programs can be installed on our guest based on our malware needs, i will go for the following programs and then take a snap and check the vms list.
```
(cuckoo-test)/opt$ vmcloak list deps
(cuckoo-test)/opt$ 
(cuckoo-test)/opt$ vmcloak install win7x64cuckoo python27  pillow adobepdf ie11 
(cuckoo-test)/opt$ vmcloak snapshot --count 1 win7x64cuckoo 192.168.56.101
(cuckoo-test)/opt$ vmcloak list vms
```
If need to increase the guests to make multiple analysis you can increase --count \*No.\*


### Create Route Table

Open (1st) new terminal to do the following commands to setup routing 
**your interface name** can be found through `ifconfig` , mine is `ens33`.
```
$ sudo sysctl -w net.ipv4.conf.vboxnet0.forwarding=1
$ sudo sysctl -w net.ipv4.conf.*your interface name*.forwarding=1
```
TO make it permenant at boot up , we can add the previous line in `/etc/sysctl.conf`
```
$ sudo nano /etc/sysctl.conf
Go to the bottom and add these lines:
net.ipv4.conf.vboxnet0.forwarding=1
net.ipv4.conf.ens33.forwarding=1

```
```
sudo iptables -t nat -A POSTROUTING -o *your interface name* -s 192.168.56.0/24 -j MASQUERADE
sudo iptables -P FORWARD DROP
sudo iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -s 192.168.56.0/24 -j ACCEPT
sudo iptables -vnL
```
This will show up, so we are good to go for cuckoo usage.
```
Chain INPUT (policy ACCEPT 24 packets, 5253 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy DROP 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         
    0     0 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    0     0 ACCEPT     all  --  *      *       192.168.56.0/24      0.0.0.0/0           

Chain OUTPUT (policy ACCEPT 24 packets, 5099 bytes)
 pkts bytes target     prot opt in     out     source               destination         

```
### Use Cuckoo
go back to our venv (cuckoo-test) open 3 terminals , and let's setup out cuckoo.
In 1st Terminal
```
(cuckoo-test)$ cuckoo init

```
Check it `ls -alh`
```
(cuckoo-test)~/.cuckoo$ cuckoo communitiy
```
### Internet Cuckoo Router

In 2nd terminal start rooter
```
$ cuckoo rooter --sudo --group cuckoo
<your current date> [cuckoo] INFO: Starting Cuckoo Rooter (group=cuckoo)!

```

In 3rd terminal choose your virtualization conf file (in our case we use Vbox)
```
$ cd .cuckoo/conf/
$ nano virtualbox.conf
```
we can manually make some changes like enable gui and so on, but just add our machine through this command then check the file again.
```
while read -r vm ip; do cuckoo machine --add $vm $ip; done < <(vmcloak list vms)
```
you can remove cuckoo1 from `machines` it doesn't exist anymore and also delete it full block

Then let's kick to `routing.conf`
```
$ nano routing.conf
```
Change your Routing option in our case `internet` to be 
```
internet = ens33
```
Then let's kick to `reporting.conf`
```
$ nano reporting.conf
```
Change your MongoDB option to be 
```
[mongodb]
enabled = yes
host = 127.0.0.1
port = 27017
db = cuckoo
store_memdump = yes
paginate = 100
# MongoDB authentication (optional).
username =
password =
```
[-][-][-][-][-][-][-][-][-][-][-][-][-][-][-][-]
- Run Guest machines (in another terminal): `VBoxManage startvm "192.168.56.1011" --type headless` 
    * Don't need to be in Cuckoo-test env

run inside Cuckoo-test env
- In our setup we access cuckoo-test via: `workon cuckoo-test`
- 1st terminal had rooter running : `cuckoo rooter --sudo --group cuckoo`
- Then 2nd  terminal run cuckoo    : `cuckoo`
- 3rd terminal run web interface  : `cuckoo web --host 127.0.0.1 --port 8080`
And Boom we are done with the host.

![Cuckoo Web](/assets/attach/Cuckoo-web.JPG)


## Cuckoo API
first things first to disable  `JSONIFY_PRETTYPRINT_REGULAR`
```
$nano /home/cuckoo/.virtualenvs/cuckoo-test/lib/python2.7/site-packages/flask/app.py
```
```py
.
.
.
    default_config = ImmutableDict({
        'DEBUG':                                get_debug_flag(default=False),
        .
        .
        .
        'PREFERRED_URL_SCHEME':                 'http',
        'JSON_AS_ASCII':                        True,
        'JSON_SORT_KEYS':                       True,
        'JSONIFY_PRETTYPRINT_REGULAR':          False,
        'JSONIFY_MIMETYPE':                     'application/json',
        'TEMPLATES_AUTO_RELOAD':                None,
    })
``` 
to see the api.py can be accessed by 
`/home/cuckoo/.virtualenvs/cuckoo-test/lib/python2.7/site-packages/cuckoo/api.py`

[-][-][-][-][-][-][-][-][-][-][-][-][-][-][-][-]
### Some errors handeling 
Also to get rid of IOError: [Errno 24] Too many open files.

If you want to increase the limit shown by `$ ulimit -n`, you should:
```
$ su cuckoo
```
Then modify `/etc/systemd/user.conf` and `/etc/systemd/system.conf` with the following line (this takes care of graphical login):

 DefaultLimitNOFILE=65535

Modify `/etc/security/limits.conf` with the following lines (this takes care of non-GUI login):

    *    soft     nproc          65535
    *    hard     nproc          65535
    *    soft     nofile         65535
    *    hard     nofile         65535
    cuckoo   soft     nproc          200000
    cuckoo   hard     nproc          200000
    cuckoo   soft     nofile         200000
    cuckoo   hard     nofile         200000

And then add following line in the file: `/etc/pam.d/common-session`

    session required pam_limits.so

Reboot your computer for changes to take effect.

### Installing Suricata 6.03
on the Host Machine , Ubuntu 
```
$ sudo add-apt-repository ppa:oisf/suricata-stable
$ sudo apt-get update
$ sudo apt-get install suricata
```
To verify `$ suricata -V`
Add permisions to suricata and let cuckoo user can use it 
```
$ sudo groupadd suricata
$ sudo chgrp -R suricata /etc/suricata
$ sudo chgrp -R suricata /var/lib/suricata/rules
$ sudo chgrp -R suricata /var/lib/suricata/update
$ sudo chown -R cuckoo:suricata suricata/
$ suricata-update
```
Then go to  `.cuckoo/conf/` dir `nano processing.conf` and `enable= yes` Suricata module 

### Some protocol not enabled
Probably by defualt Suricata not enabled the following protocols and will POP up an error during analysis:
- RDP
- SIP

just go to `/etc/suricata` and `sudo nano suricata.yaml`
Find this section and edit it to be as following:
```
app-layer:
  protocols:

    rfb:
      enabled: yes
      detection-ports:
        dp: 5900, 5901, 5902, 5903, 5904, 5905, 5906, 5907, 5908, 5909
    # MQTT, disabled by default.
    mqtt:
      enabled: no
      # max-msg-length: 1mb
      # subscribe-topic-match-limit: 100
      # unsubscribe-topic-match-limit: 100
    krb5:
      enabled: yes
    snmp:
      enabled: yes
    ikev2:
      enabled: yes
    tls:
    .
    .
    .
    .
     ftp:
      enabled: yes
      # memcap: 64mb
    rdp:
      enabled: yes
    ssh:
      enabled: yes
      #hassh: yes
    # HTTP2: Experimental HTTP 2 support. Disabled by default.
    http2:
      enabled: no
    .
    .
    .
   ntp:
      enabled: yes

    dhcp:
      enabled: yes

    sip:
      enabled: yes


```
## Malware Selection

As a rat test you can go for [eicar](https://www.eicar.org/download-anti-malware-testfile/)
and download eicar_com.zip then drag and drop it ,choose internal file and start analysis.

This part is kind sus , takecare and be aware of what malware you use to can figure out your cuckoo analysis and capture it working well.

For Malware database we can go for [MalwareBazaar](https://bazaar.abuse.ch/) , there's [any.run](https://app.any.run/submissions?_gl=1*8pso38*_ga*ODk2NzgxMTI5LjE2NTA4Mjk2MTg.*_ga_53KB74YDZR*MTY1MTA0MDEyMS4yLjEuMTY1MTA0MDE1Mi4yOQ..&_ga=2.211332950.2109364546.1651040122-896781129.1650829618/)

Choose your sample , get to know it and download it in Cuckoo host.

Make sure guest is up , malware is ready and you locate in cuckoo enviroment and start analysis by:
1- Run Cuckoo for guest 
2- Run Cuckoo web
3- Sumbit malware/URL/hash needed to be analysised on the guest.

+++++++++++++++++++++++++++++++++++++++++++++++++++
