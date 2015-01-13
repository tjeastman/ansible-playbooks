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

Create and configure a Hadoop cluster in EC2 with three datanodes:
```
ansible-playbook aws-hadoop-cluster-create.yml -e datanodes=3 -e username=username
ansible-playbook -u ubuntu -i inventory/aws hadoop-cluster-config.yml
./inventory/aws --list
```

Format the namenode:
```
cd /usr/local/hadoop
./bin/hdfs namenode -format
```

Setup Ganglia:
```
ansible-playbook -i inventory/aws ganglia-client-setup.yml -e 'ganglia_cluster_name="cluster name"' -e ganglia_host=hostname
ansible-playbook -i inventory/aws ganglia-server-setup.yml -l namenode -e 'ganglia_cluster_name="cluster name"' -e ganglia_host=hostname
```
