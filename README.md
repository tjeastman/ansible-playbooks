Run shell commands on the host with ansible:
```
ansible -i hosts.txt -a whoami all
ansible -i hosts.txt -a whoami all --sudo
```
