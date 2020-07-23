# OpenShift 4.5 vSphere Installer Provisioned Infrastructure Example
The release of OpenShift 4.5 added a new vSphere IPI installation option that makes it very easy to quickly spin up an OCP cluster in an EXSi environment.  The "straight" out of the box installation creates three masters and three worker nodes with minimal effort.  IPI installation supports additional customizations, but in this example I will not use any of the customization capabilities.

The lab I'm using for this installation is made up of three x86 8-core 64GB RAM machines formally used for gaming.  EXSI environment is a bare bones VMWare vSphere Essentials 6.7 setup.  I'm also using a two bay Synology NAS for shared storage across the vSphere cluster.  

OCP 4.5 installation documentation can be found here -> https://docs.openshift.com/container-platform/4.5/welcome/index.html

## Installation Steps

###Installation Pre-reqs:
For this OCP 4.5 IPI vSphere installation, you need DNS and DHCP available to the cluster.
- DNS service - For the installation you need to define two static IP address.  One for the cluster api access - api.ocp4.example.com and one for cluster ingress access *.apps.ocp4.example.com. For my lab I use example.com as the domain.
  - Forward zone settings
    - api.ocp4	IN	A	10.1.10.181
    - *.apps.ocp4	IN	A	10.1.10.182
  - Reverse zone settings
    - api.ocp4	A	10.1.10.181
    - *.apps.ocp4	A	10.1.10.182
    - 181	IN	PTR	api.ocp4.example.com.
- DHCP service

### Optional - Create an ssh key for password-less ssh to the ssh to master node for debugging, etc.
  1. Create ssh key 
    - ssh-keygen -t rsa -b 4096 -N '' -f ~/.ssh/ocp45
  ![SSH command](/images/SSHKey01.jpg)
  
  2. Start up ssh-agent and add the new key to the ssh-agent.  This key will be added during the installation.
    - eval "$(ssh-agent -s)"
    - ssh-add .ssh/ocp45
  ![SSH agent](/images/ssh02.jpg)


