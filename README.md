#Unicorn-RHEL demo instructions

With your Red Hat email address, join the Ansible Tower Demo Slack Team https://ansibletowerdemo.slack.com and go to #tower_notify channel to see notifications from your demo job runs.

1. Run Job Template __1 - Project Unicorn-RHEL - Launch AWS RHEL7 Instances needed for Unicorn__
   * Select __Username Inventory__ when prompted.
2. Run Job Template __2 - Project Unicorn-RHEL: Install Apps and Setup Web Servers__
   * Select __Username Inventory__ when prompted.
3. Run Job Template __3 - Project Unicorn-RHEL: Enable SSI on Apache__
   * Select __Username Inventory__ when prompted.
4. Go to __Inventory Tab__
   * Select __INVENTORIES / Username INVENTORY / AP-SOUTHEAST-1 / TAGS / TAG_ANSIBLE_GROUP / TAG_ANSIBLE_GROUP_LB__
   * Copy the __IP Address__ appearing on the right side (under HOSTS)
   * Paste the __IP Address__ on a new Browser window and __add :8888__ at the end
   * Click Enter to go __HAProxy load-balancer__ webpage at x.x.x.x:8888 
   * __Refresh Browser__ window to see Round-Robin load-balancing in effect (note the IP address changing)
5. Run Job Template __4 - Project Unicorn-RHEL: Performs a Rolling Update__
   * __Refresh Browser__ window to check progress of rolling update
6. Run Job Template __5 - Project Unicorn-RHEL: Delete all AWS RHEL Instances__

## Running on command line

### Inventory
* Hosts to be in one of the 3 following groups
 * tag_ansible_group
 * tag_ansible_group_webservers
 * tag_ansible_group_lbservers

#### Example
```
   [tag_ansible_group]
   centos7
   rhel7_lamp

   [tag_ansible_group_webservers]
   rhel7_lamp

   [tag_ansible_group_lbservers]
   centos7
```

### site.yaml
* Requires variable
 * ntpserver 
 * mode - for haproxy operating mode
 * daemonname - for haproxy
 * listenport - for haproxy
 * balance - for haproxy

#### Example
```
    ansible-playbook -i ../unicorn.hosts \
    -e "ntpserver=ntp1.orcon.net.nz mode=http daemonname=myapplb listenport=8888 balance=roundrobin\
    iface='{{ ansible_default_ipv4.interface }}' \
    repository='https://github.com/nicholas-chia/mywebapp.git' \
    httpd_port=80" \
    site.yml 
```

#### Example Variables
    balance: roundrobin
    daemonname: myapplb
    httpd_port: 80
    iface: '{{ ansible_default_ipv4.interface }}'
    listenport: 8888
    mode: http
    ntpserver: 192.168.1.2
    repository: 'https://github.com/nicholas-chia/mywebapp.git'

### enablessl.yaml
Enables SSL on Apacke


### rolling_update.yml

