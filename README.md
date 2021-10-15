# configure_environments

This demos the setup of ansible on a linux RHEL environment which can be used for automating the software provisioning or configure environments with prerequisite setup of softwares on a test/build environments. This sample code demos how to configure the system environment variables, modify the registries and install the prerequisite softwares conveniently on a remote windows platform in a quick timespan. This code can be extended to have support for other platforms as well.

Current sample code supports for below platforms:
1. windows 10
2. windows server 16
3. windows server 19

Usage:
1. Install python 3.6.8
2. Install python3-virtualenv
3. Create a virtual envrionment
4. Install ansible in the virtual environment
5. clone this repo to your etc/ansible directory
6. Update the hosts file /etc/ansible/hosts with the details of remote host machines that you are required to perform software provisioning /configure environments 
7. You are ready to GO!!!

Detailed Instructions:
Step 1: Verify that Python3 is installed on Ansible control node
# sudo dnf install python3
# sudo alternatives --set python /usr/bin/python3
Step 2: Create a virtual environment to begin with.
# sudo dnf install python3-virtualenv
# virtualenv env-autospinup
Using base prefix '/usr'
New python executable in /root/env-autospinup/bin/python3.6
Also creating executable in /root/ansiblecicd/bin/python
Installing setuptools, pip, wheel...done.
[<user>@oransicentos8 ~]# ls
anaconda-ks.cfg  env-autospinup  env  initial-setup-ks.cfg 

[<user>@oransicentos8 ~]# source env-autospinup/bin/activate

(env-autospinup) [<user>@oransicentos8 ~]$ python --version
Python 3.6.8

(env-autospinup) [<user>@oransicentos8 ~]# pip install ansible
Collecting ansible
  Downloading ansible-2.9.7.tar.gz (14.2 MB)
     |████████████████████████████████| 14.2 MB 5.1 MB/s 
Collecting jinja2
  Downloading Jinja2-2.11.2-py2.py3-none-any.whl (125 kB)
     |████████████████████████████████| 125 kB 78.1 MB/s 
Collecting PyYAML
  Downloading PyYAML-5.3.1.tar.gz (269 kB)
     |████████████████████████████████| 269 kB 61.0 MB/s 
Collecting cryptography
  Downloading cryptography-2.9.2-cp35-abi3-manylinux2010_x86_64.whl (2.7 MB)
     |██████████████████████████████▌ | 2.6 MB 76.6 MB/s eta 0:00:01
Check the ansible version after install and make sure the config file path is mapped.

(env-autospinup) [akarunak@oransicentos8 ~]$ ansible --version
ansible 2.9.7
config file = /etc/ansible/ansible.cfg
configured module search path = ['/home/<user>/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
ansible python module location = /home/<user>/env-autospinup/lib/python3.6/site-packages/ansible
executable location = /home/<user>/env-autospinup/bin/ansible
python version = 3.6.8 (default, Nov 21 2019, 19:31:34) [GCC 8.3.1 20190507 (Red Hat 8.3.1-4)]

If in case config file = None, then follow the below steps to set up the ansible.cfg and hosts. host file is required to configure the host servers that you would like to establish connections with.

run the command,

$ ansible-config view

Clone this repo to /etc/ansible directory

Run the command to check Ansible setup is complete,

sudo systemctl status sshd
And check whether the sshd connection is Active and in running state and check whether the session is opened with permissions enabled for user root .

If permission are not enabled and fails for root user with below error,

May 12 20:15:17 oransicentos8 sshd[31140]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=10.5.20.99 user=root
May 12 20:15:17 oransicentos8 sshd[31140]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
May 12 20:15:19 oransicentos8 sshd[31140]: Failed password for root from 10.5.20.99 port 29101 ssh2
May 12 20:15:19 oransicentos8 sshd[31140]: Connection closed by authenticating user root 10.5.20.99 port 29101 [preauth]
May 12 20:15:31 oransicentos8 sshd[31143]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=10.5.20.99 user=root
May 12 20:15:31 oransicentos8 sshd[31143]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
May 12 20:15:33 oransicentos8 sshd[31143]: Failed password for root from 10.5.20.99 port 29139 ssh2
May 12 20:15:35 oransicentos8 sshd[31143]: Connection closed by authenticating user root 10.5.20.99 port 29139 [preauth]
May 13 03:31:27 oransicentos8 systemd[1]: Stopping OpenSSH server daemon...
May 13 03:31:27 oransicentos8 systemd[1]: Stopped OpenSSH server daemon.
Restart the systemctl sshd service,

[<user>@oransicentos8 ~]$ sudo systemctl start sshd
[<user>@oransicentos8 ~]$ sudo systemctl enable sshd
And Wait a while and make sure the pam_unix(sshd:session): session opened for user

(env-autospinup) [<user>@oransicentos8 ~]$ sudo systemctl status sshd
[sudo] password for <user>:
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset>
   Active: active (running) since Wed 2020-05-13 03:31:48 EDT; 21min ago
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 5901 (sshd)
    Tasks: 1 (limit: 26213)
   Memory: 3.4M
   CGroup: /system.slice/sshd.service
           └─5901 /usr/sbin/sshd -D -oCiphers=aes256-gcm@openssh.com,chacha20-p>

May 13 03:31:48 oransicentos8 systemd[1]: Starting OpenSSH server daemon...
May 13 03:31:48 oransicentos8 sshd[5901]: Server listening on 0.0.0.0 port 22.
May 13 03:31:48 oransicentos8 sshd[5901]: Server listening on :: port 22.
May 13 03:31:48 oransicentos8 systemd[1]: Started OpenSSH server daemon.
May 13 03:52:48 oransicentos8 sshd[6649]: Accepted password for <user> from 1>
May 13 03:52:48 oransicentos8 sshd[6649]: pam_unix(sshd:session): session opene>
lines 1-17/17 (END)
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2020-05-13 03:31:48 EDT; 21min ago
     Docs: man:sshd(8)
           man:sshd_config(5)
 Main PID: 5901 (sshd)
    Tasks: 1 (limit: 26213)
   Memory: 3.4M
   CGroup: /system.slice/sshd.service
           └─5901 /usr/sbin/sshd -D -oCiphers=aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes256-ctr,aes256-cbc,aes128-gcm@openssh.com,aes128-ctr,aes1>

May 13 03:31:48 oransicentos8 systemd[1]: Starting OpenSSH server daemon...
May 13 03:31:48 oransicentos8 sshd[5901]: Server listening on 0.0.0.0 port 22.
May 13 03:31:48 oransicentos8 sshd[5901]: Server listening on :: port 22.
May 13 03:31:48 oransicentos8 systemd[1]: Started OpenSSH server daemon.
May 13 03:52:48 oransicentos8 sshd[6649]: Accepted password for <user> from 10.5.248.139 port 60719 ssh2
May 13 03:52:48 oransicentos8 sshd[6649]: pam_unix(sshd:session): session opened for user <user> by (uid=0)

create ssh-keygen,

cd /etc/ansible
$ ssh-keygen
[<user>@oransicentos8 ansible]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/<user>/.ssh/id_rsa): 
/home/<user>/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/<user>/.ssh/id_rsa.
Your public key has been saved in /home/<user>/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:WZsirFvp+82Mcwaiz67j99KGOfw/UKAOP8ZsS3NTJF8 <user>@oransicentos8
The key's randomart image is:
+---[RSA 3072]----+
| |
| o . E |
| . =.. |
| ... o+o |
| *o Soo |
| .%o=. |
| .*oX + |
| o=O +=+ |
| .+**O===. |
+----[SHA256]-----+
To copy the generated SSH key to the remote node run the below command,

$ ssh-copy-id <user@unixhost_ipaddress>
Check whether your ping to the local machine is working fine without any issues as below,

(env-autospinup) [<user>@oransicentos8 ~]$ ansible localhost -m ping
localhost | SUCCESS => {
"changed": false,
"ping": "pong"
}
Recheck on inventory host values specified by the user for remote server configurations,

 ansible-inventory --list
Should return the below JSON format if hosts are specified,
{
"_meta": {
"hostvars": {
"<host_ip1>": {
"ansible_password": "<password>",
"ansible_user": "<username>",
},
"<host_ip2>": {
"ansible_password": "<password>",
"ansible_user": "<username>",
}
}
},
"all": {
"children": [
"ungrouped",
"linuxhost"
]
},
"linuxhost": {
"hosts": [
"<host_ip1>",
"<host_ip2>"
]
}
}
Now use ansible to ping the remote hosts and check whether connection is established between the ansible server and remote windows host machine,

(env-autospinup) [<user>@oransicentos8 bin]$ ansible -i /etc/ansible/hosts -m ping all
<host_ip1> | SUCCESS => {
"changed": false,
"ping": "pong"
}
<host_ip2> | SUCCESS => {
"changed": false,
"ping": "pong"
}
  
Step 2: Configuring Setup for managing Windows hosts using ansible
Part1: Configure Control Machine
$ pip install pyOpenSSL --upgrade
Install pywinrm with support for basic, certificate, and NTLM auth, simply

$ pip install pywinrm
(env-autospinup) [<user>@oransicentos8 bin]$ pip install pywinrm
Part2: Configuring Windows Host
For configuring our Windows 10 remote host system to connect with the Ansible Control node. We are going to install the WinRM listener- short for Windows Remote – which will allow the connection between the Windows host system and the Ansible server.

Before we do so, the Windows host system needs to fulfill a few requirements for the installation to succeed:

Your Windows host system should be Windows 7 or later. For Servers, ensure that you are using Windows Server 2008 and later versions.
Ensure your system is running .NET Framework 4.0 and later.
Windows PowerShell should be Version 3.0 & later
With all the requirements met, now follow the steps stipulated below:

Download the ConfigureRemotingForAnsible.ps1 file to the desktop of the remote windows host VM and run it using powershell 3.0 or greater version as an administrator

.\ConfigureRemotingForAnsible.ps1
Make sure a self signed SSL certificate is generated.
  
Part3:  Check for connection
And perform the below steps,

cd /etc/ansible/
update hosts file,

[winhost] 
<serverip1> 
<serverip2>
[winhost:vars] 
ansible_user=<username> 
ansible_password=<password> 
ansible_connection=winrm 
ansible_winrm_server_cert_validation=ignore
Run the below command to verify whether you are able to ping to hostservers from ansible,

$ ansible winhost -m win_ping
(env-autospinup) [akarunak@oransicentos8 ~]$ ansible winhost -m win_ping
<host_ip1> | SUCCESS => {
"changed": false,
"ping": "pong"
}
<host_ip2> | SUCCESS => {
"changed": false,
"ping": "pong"
}
Note*: ‘win_ping’ is used here for windows host connections, if it is unix host machine that we are trying to ping, then we are suppose to use ‘ping’ instead of win_ping.

Based on title specified in the hosts file you can ping both windows hosts and unix hosts continuously,
$ ansible -i /etc/ansible/hosts linuxhost -m ping
$ ansible -i /etc/ansible/hosts winhost -m win_ping
Part4: Try running remote commands
$ ansible -i /etc/ansible/hosts winhost -m win_command -a "cmd /c dir C:\\"
(env-autospinup) [<user>@oransicentos8 ~]$ ansible -i /etc/ansible/hosts winhost -m win_command -a "cmd /c dir C:\\"
<host_ip1> | CHANGED | rc=0 >>
Volume in drive C has no label.
Volume Serial Number is 2A92-F6EB

Directory of C:\

05/08/2020 02:08 AM <DIR> cygwin64
04/30/2020 05:43 AM <DIR> Jenkins
09/15/2018 12:19 AM <DIR> PerfLogs
05/08/2020 12:43 PM <DIR> Program Files
05/08/2020 12:56 PM <DIR> Program Files (x86)
03/26/2020 01:22 AM <DIR> Users
05/08/2020 01:04 PM <DIR> Windows
0 File(s) 0 bytes
7 Dir(s) 186,558,099,456 bytes free
$ ansible -i /etc/ansible/hosts linuxhost -m command -a "sudo apt-get install vim"
Part5: Configuring Kerberos & Credssp
Alternatively, one can use Kerberos authentication for a strong authentication for client/server applications by using secret-key cryptography. You need these optional dependencies,

$ sudo yum install gcc python3-devel krb5-devel krb5-workstation python3-devel
Then Install Kerberos,

$ sudo pip3 install "pywinrm[kerberos]"
(env-autospinup) [<user>@oransicentos8 ~]$ sudo pip3 install "pywinrm[kerberos]"
WARNING: pip is being invoked by an old script wrapper. This will fail in a future version of pip.
Please see https://github.com/pypa/pip/issues/5599 for advice on fixing the underlying issue.
To avoid this problem you can invoke Python with '-m pip' instead of running pip directly.
Requirement already satisfied: pywinrm[kerberos] in /usr/local/lib/python3.6/site-packages (0.4.1)
Requirement already satisfied: xmltodict in /usr/local/lib/python3.6/site-packages (from pywinrm[kerberos]) (0.12.0)
Requirement already satisfied: requests>=2.9.1 in /usr/lib/python3.6/site-packages (from pywinrm[kerberos]) (2.20.0)
Requirement already satisfied: requests_ntlm>=0.3.0 in /usr/local/lib/python3.6/site-packages (from pywinrm[kerberos]) (1.1.0)
Requirement already satisfied: six in /usr/lib/python3.6/site-packages (from pywinrm[kerberos]) (1.11.0)
Collecting pykerberos<2.0.0,>=1.2.1
Using cached pykerberos-1.2.1.tar.gz (24 kB)
Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /usr/lib/python3.6/site-packages (from requests>=2.9.1->pywinrm[kerberos]) (3.0.4)
Requirement already satisfied: idna<2.8,>=2.5 in /usr/lib/python3.6/site-packages (from requests>=2.9.1->pywinrm[kerberos]) (2.5)
Requirement already satisfied: urllib3<1.25,>=1.21.1 in /usr/lib/python3.6/site-packages (from requests>=2.9.1->pywinrm[kerberos]) (1.24.2)
Requirement already satisfied: ntlm-auth>=1.0.2 in /usr/local/lib/python3.6/site-packages (from requests_ntlm>=0.3.0->pywinrm[kerberos]) (1.4.0)
Requirement already satisfied: cryptography>=1.3 in /usr/lib64/python3.6/site-packages (from requests_ntlm>=0.3.0->pywinrm[kerberos]) (2.3)
Requirement already satisfied: asn1crypto>=0.21.0 in /usr/lib/python3.6/site-packages (from cryptography>=1.3->requests_ntlm>=0.3.0->pywinrm[kerberos]) (0.24.0)
Requirement already satisfied: cffi!=1.11.3,>=1.7 in /usr/lib64/python3.6/site-packages (from cryptography>=1.3->requests_ntlm>=0.3.0->pywinrm[kerberos]) (1.11.5)
Requirement already satisfied: pycparser in /usr/lib/python3.6/site-packages (from cffi!=1.11.3,>=1.7->cryptography>=1.3->requests_ntlm>=0.3.0->pywinrm[kerberos]) (2.14)
Building wheels for collected packages: pykerberos
Building wheel for pykerberos (setup.py) ... done
Created wheel for pykerberos: filename=pykerberos-1.2.1-cp36-cp36m-linux_x86_64.whl size=79357 sha256=529d8e61e7e35272591ab822f928c2f8953dfd11ddabed1869e293af071e825f
Stored in directory: /root/.cache/pip/wheels/8b/04/7b/a655ef6a54543ae28ba60f30ced4a03fd4c77f4bc46b3e965a
Successfully built pykerberos
Installing collected packages: pykerberos
Successfully installed pykerberos-1.2.1
Install credssp,

$ sudo pip3 install "pywinrm[credssp]"
(env-autospinup) [<user>@oransicentos8 ~]$ sudo pip3 install "pywinrm[credssp]"
WARNING: pip is being invoked by an old script wrapper. This will fail in a future version of pip.
Please see https://github.com/pypa/pip/issues/5599 for advice on fixing the underlying issue.
To avoid this problem you can invoke Python with '-m pip' instead of running pip directly.
Requirement already satisfied: pywinrm[credssp] in /usr/local/lib/python3.6/site-packages (0.4.1)
Requirement already satisfied: xmltodict in /usr/local/lib/python3.6/site-packages (from pywinrm[credssp]) (0.12.0)
Requirement already satisfied: requests>=2.9.1 in /usr/lib/python3.6/site-packages (from pywinrm[credssp]) (2.20.0)
Requirement already satisfied: requests_ntlm>=0.3.0 in /usr/local/lib/python3.6/site-packages (from pywinrm[credssp]) (1.1.0)
Requirement already satisfied: six in /usr/lib/python3.6/site-packages (from pywinrm[credssp]) (1.11.0)
Collecting requests-credssp>=1.0.0
Downloading requests_credssp-1.1.1-py2.py3-none-any.whl (20 kB)
Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /usr/lib/python3.6/site-packages (from requests>=2.9.1->pywinrm[credssp]) (3.0.4)
Requirement already satisfied: idna<2.8,>=2.5 in /usr/lib/python3.6/site-packages (from requests>=2.9.1->pywinrm[credssp]) (2.5)
Requirement already satisfied: urllib3<1.25,>=1.21.1 in /usr/lib/python3.6/site-packages (from requests>=2.9.1->pywinrm[credssp]) (1.24.2)
Requirement already satisfied: ntlm-auth>=1.0.2 in /usr/local/lib/python3.6/site-packages (from requests_ntlm>=0.3.0->pywinrm[credssp]) (1.4.0)
Requirement already satisfied: cryptography>=1.3 in /usr/lib64/python3.6/site-packages (from requests_ntlm>=0.3.0->pywinrm[credssp]) (2.3)
Requirement already satisfied: pyOpenSSL>=16.0.0 in /usr/lib/python3.6/site-packages (from requests-credssp>=1.0.0->pywinrm[credssp]) (18.0.0)
Collecting pyasn1>=0.3.1
Downloading pyasn1-0.4.8-py2.py3-none-any.whl (77 kB)
|████████████████████████████████| 77 kB 5.1 MB/s
Requirement already satisfied: asn1crypto>=0.21.0 in /usr/lib/python3.6/site-packages (from cryptography>=1.3->requests_ntlm>=0.3.0->pywinrm[credssp]) (0.24.0)
Requirement already satisfied: cffi!=1.11.3,>=1.7 in /usr/lib64/python3.6/site-packages (from cryptography>=1.3->requests_ntlm>=0.3.0->pywinrm[credssp]) (1.11.5)
Requirement already satisfied: pycparser in /usr/lib/python3.6/site-packages (from cffi!=1.11.3,>=1.7->cryptography>=1.3->requests_ntlm>=0.3.0->pywinrm[credssp]) (2.14)
Installing collected packages: pyasn1, requests-credssp
Successfully installed pyasn1-0.4.8 requests-credssp-1.1.1
  
