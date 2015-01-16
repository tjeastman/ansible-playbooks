Create a Hadoop cluster in EC2:
```
ansible-playbook aws-hadoop-cluster-create.yml -e created_by=username -e key_name=key_name
```
The cluster will have one namenode and three datanodes by default (use namenodes=n and/or datanodes=m to override).
The EC2 instances and related AWS resources that are created by this playbook will be tagged based on the value
of "created_by" in order to distinguish them from resources created by other users.
Note that "key_name" should indicate the name of a key pair available to you in the AWS console.
This key should also be available locally in order for ansible to establish SSH connections to the EC2 nodes when executing
other playbooks.
See the contents of aws-hadoop-cluster-create.yml for additional configuration variables.

There is a dynamic inventory script that can be used to view the list of EC2 instances:
```
./inventory/aws --list
```

Install some required packages and then install and configure Hadoop.
```
ansible-playbook -u ubuntu -i inventory/aws packages-base.yml
ansible-playbook -u ubuntu -i inventory/aws hadoop-install.yml
ansible-playbook -u ubuntu -i inventory/aws hadoop-configure.yml
```
Note that the Hadoop 2.5.2 binary distribution should be available in the local download directory (download/hadoop-2.5.2.tar.gz).

The cluster creation playbook uses a standard Ubuntu AMI.
This AMI provides a user named ubuntu with superuser privileges but you may wish to create you own user account:
```
ansible-playbook -u ubuntu -i inventory/aws -e username=username -e shell=/path/to/sh -e public_key=/path/to/public_key -e '{"groupnames":["hadoop","sudo","group","other_group"]}' add-superuser.yml
```
The group hadoop is created when Hadoop is installed above.
Users in this group can start and stop Hadoop services without root sudo.

Some services on the Hadoop stack depend on being able to SSH between hosts without a password.
The following playbook will copy SSH keys onto the cluster nodes
```
ansible-playbook -i inventory/aws -e private_key=/path/to/private_key -e public_key=/path/to/public_key copy-ssh-keys.yml
```
Note that the private key should not be encrypted.

To start running Hadoop, first log in to the namenode and format HDFS:
```
cd /usr/local/hadoop
./bin/hdfs namenode -format
```
Then start Hadoop on the cluster:
```
ansible-playbook -i inventory/aws hadoop-start.yml
```

There is a playbook that will destroy the cluster nodes and associated resources in AWS if desired:
```
ansible-playbook aws-hadoop-cluster-destroy.yml -e created_by=username
```
