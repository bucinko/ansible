
# Ansible tutorial: Inventory

The default place for inventory file is `/etc/ansible/hosts`
However, you can configure ansible to look :

 - use an environment variable (`ANSIBLE_HOSTS`)
 - use the `-i` flag in ansible commands an provide the inventory path.

```bash
[loadbalancer]
lb01            ansible_host=vm0        ansible_user=azureuser
[web]
app01           ansible_host=vm1        ansible_user=azureuser
[database]
db01            ansible_host=vm2        ansible_user=azureuser
```

`ansible_host` is a special _variable_ that sets the hostname/ip adress ansible will use when
trying to connect to this host.

`ansible_user` is another special _variable_ that tells ansible to
connect as this user when using ssh.

### Host grouping

Hosts in inventory can be grouped arbitrarily. For instance, we have load balancer, app group etc
So we can add more hosts to group and configure them accordingly

we can use also child group  define with [groupname:children]
```bash
[production:children]
loadbalancer
web
database
```

we can disable host key checking by adding file vi ~/.ansible.cfg

```bash
[defaults]
host_key_checking = False
```


### Testing connection

What ansible will try to do here is just executing the ping module (more on modules later) on each host.
The output should look like this:

```bash
ansible -m ping -i 01-inventory/hosts  all
```

```bash
lb01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
db01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
app01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```


### Selecting hosts

we can test all hosts by parameter ```all```

We can also test conection to one node by calling its name e.g ```app01```   

```bash
ansible -m ping -i 01-inventory/hosts  app01
```

Test conection to specific group of servers 'webserver'


```bash
ansible -m ping -i 01-inventory/hosts  webserver
```