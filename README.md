Run shell commands on the host with ansible:
```
ansible -i hosts.txt -a whoami all
ansible -i hosts.txt -a whoami all --sudo
```

Install Debian packages:
```
ansible-playbook -i hosts.txt main.yml --sudo
```

Set hostname for a new VM:
```
ansible-playbook -i hosts.txt hostname.yml -e hostname=new-host --sudo
```

Install personal configs:
```
ansible-playbook -i hosts.txt personal.yml -e name=name -e email=email -k
```

Create a Hadoop cluster in EC2 with three datanodes:
```
ansible-playbook hadoop-cluster-create.yml -e datanodes=3
```
