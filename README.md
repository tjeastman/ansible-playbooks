Create and configure a Hadoop cluster in EC2 with three datanodes:
```
ansible-playbook aws-hadoop-cluster-create.yml -e created_by=username -e key_name=key_name
ansible-playbook -u ubuntu -i inventory/aws -e username=username -e shell=/path/to/sh -e public_key=/path/to/pubkey -e '{"groupnames":["group1","group2","group3"]}' add-superuser.yml
ansible-playbook -i inventory/aws packages-base.yml
```
Ensure that the correct SSH key is available to allow ansible to connect to
hosts in AWS for both the default ubuntu user and the user created above.

View the list of EC2 instances:
```
./inventory/aws --list
```

Copy SSH keys onto the cluster nodes (note that the private key should not be encrypted):
```
ansible-playbook -i inventory/aws -e private_key=/path/to/prikey -e public_key=/path/to/pubkey copy-ssh-keys.yml
```

Setup Ganglia:
```
ansible-playbook -i inventory/aws ganglia-client-setup.yml -e 'ganglia_cluster_name="cluster name"' -e ganglia_host=hostname
ansible-playbook -i inventory/aws ganglia-server-setup.yml -l namenode -e 'ganglia_cluster_name="cluster name"' -e ganglia_host=hostname
```

Install and configure Hadoop:
```
ansible-playbook -i inventory/aws hadoop-install.yml
ansible-playbook -i inventory/aws hadoop-configure.yml -e ganglia_host=hostname
```

Log in to the namenode and format HDFS:
```
cd /usr/local/hadoop
./bin/hdfs namenode -format
```

Start Hadoop on the cluster:
```
ansible-playbook -i inventory/aws hadoop-start.yml
```

To destroy the cluster nodes and associated resources in AWS:
```
ansible-playbook aws-hadoop-cluster-destroy.yml -e created_by=username
```
