## What is the Goal Here?

Text here

## What is Cloud-init?

According to the official Ubuntu Documentation website, cloud-init is an Ubuntu package that “handles early initialization of a cloud instance”. It is used to apply a predetermined set of configurations to cloud instances. More on CloudInit can be found [here](https://help.ubuntu.com/community/CloudInit).

## Operating Systems and Tools Used

The following were used in this task:
- [Ubuntu Server 20.04.3 LTS](https://ubuntu.com/download/server)
- [VMware Workstation 16 Pro](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)
- [whois](https://www.tecmint.com/whois-command-get-domain-and-ip-address-information/), [mkpasswd](https://linux.die.net/man/1/mkpasswd)
- [OpenSSH](https://ubuntu.com/server/docs/service-openssh), [ssh-keygen](https://www.ssh.com/academy/ssh/keygen)

## Master Server Setup

The assumption for this section is that you are familiar with how to install Ubuntu Server 20.04.3 LTS or any Ubuntu Linux distro.

The following steps show how to setup the master server for this task:
 Log into the VM
1. Use ```cd ~``` to navigate into the home directory.
2. Use the ```mkpasswd``` utility to make a hashed and salted password.
3. Use the ```ssh-keygen``` utility to generate a public RSA ssh key.
4. Use ```mkdir cloud-config``` to make a ```cloud-config``` directory.
5. Use ```cd cloud-config``` to cd into the ```cloud-config``` directory you just made.
6. Use ```nano user-data``` to create and open a file called ```user-data``` for editing.

- Enter the following into the file:

  ```
  #cloud-config
  autoinstall:
    version: 1
    early-commands:
      - systemctl stop ssh # otherwise packer tries to connect and exceed max attempts
    network:
      network:
        version: 2
        ethernets:
          eth0:
            dhcp4: yes
            dhcp-identifier: mac
    apt:
      preserve_sources_list: false
      primary:
        - arches: [amd64]
          uri: "http://archive.ubuntu.com/ubuntu/"
    ssh:
      install-server: yes
      authorized-keys:
        - ssh-rsa your_public_key
      allow-pw: no
    identity:
      hostname: ubuntu-01
      password: "your hashed passwd" # root
      username: your_username # root doesn't work
    packages:
      - open-vm-tools
    user-data:
      disable_root: false 
    late-commands:
      - echo 'ubuntu ALL=(ALL) NOPASSWD:ALL' > /target/etc/sudoers.d/ubuntu
      - sed -ie 's/GRUB_CMDLINE_LINUX=.*/GRUB_CMDLINE_LINUX="net.ifnames=0 ipv6.disable=1 biosdevname=0"/' /target/etc/default/grub
      - curtin in-target --target /target update-grub2
  ```




## Cloud-init Config Files Setup

Text here

## HTTP Server Setup

Text here

## FTP Server Setup

Text here

## Slave Server Setup to Use Cloud-init

Text here

## Slave Server Testing

Text here

## Results and Discussion

Text here

## Troubleshooting

Text here

## Recommendations

Text here

## Sources
