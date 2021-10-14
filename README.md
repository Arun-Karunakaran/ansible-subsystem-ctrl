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
5. clone this repo to your etc directory
6. Update the hosts file /etc/ansible/hosts with the details of remote host machines that you are required to perform software provisioning /configure environments 
7. You are ready to GO!!!
