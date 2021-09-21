## What is the Goal Here?

Text here

## What is Cloud-init?

According to the official Ubuntu Documentation website, cloud-init is an Ubuntu package that “handles early initialization of a cloud instance”. It is used to apply a predetermined set of configurations to cloud instances. More on CloudInit can be found [here](https://help.ubuntu.com/community/CloudInit).

## Operating Systems and Tools Used

The following were used in this task:
- Ubuntu Server 20.04.3 LTS
- VMware Workstation 16 Pro

## Master Server Setup

The assumption for this section is that you are familiar with how to install Ubuntu Server 20.04.3 LTS or any Ubuntu Linux distro.

The following steps show how to setup the master server for this task:
 Log into the VM
Use ```Cd ~``` to navigate into the home directory
Use the “mkpasswd” command to make a hashed and salted password
Use the “ssh-keygen” utility to generate a public RSA ssh key
Use ```mkdir cloud-config``` to make a ```cloud-config``` directory
Use ```Cd cloud-config``` to cd into the ```cloud-config``` directory you just made
Use ```nano user-data``` to create and open a file called ```user-data``` for editing.

Enter the following into the file:

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



You can use the [editor on GitHub](https://github.com/christianhacks/cloud-init.io/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/christianhacks/cloud-init.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
