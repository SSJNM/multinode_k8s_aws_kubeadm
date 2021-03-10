One Tap Multi-Node Kubernetes Setup on AWS Cloud
=========

The final set of codes for fully configuring A Master Node and Slave Nodes is here.
Just have to follow some easy steps and after that a final enter to the ansible-playbook command will do the trick.

Requirements
------------

The setup needs three Roles [ssjnm.master_slave_ec2], [ssjnm.kubeadm_aws_master] and [ssjnm.kubeadm_aws_slaves] and their requirements needs to be satisfied. 
Every detail related to roles is attached with the role itself so click on the Hyperlink to know about variable and thier defaults  


Algorithm
-------------
- First of all fill up the needed fields in vars file in all the three roles
- Do or Don't make any changes in the playbook file
- Make sure your AWS credentials are exported in the form of environmental variables
- Copy the aws private key into credentials folder with chmod 600 permissions
- Run the playbook with *ansible-playbook playbook.yml --private-key credentials/<your_private_key>.pem * in the working directory

Explanation
==========

The setup involves playing a multi-playbook file which is making use of static and dynamic inventories to trace the public IP of Master and Slave Nodes that have been launched 

Play-1: Playbook for launching Master and slave nodes
--------------

For Successful Run of this play , We need to define variables in [ssjnm.master_slave_ec2] roles folder which is already explained in depth in role site


aws_master_key_name: aws_cloud_key

aws_slaves_key_name: aws_cloud_key

your_subnet_id: subnet-1234abcd


Play-2: Making entries to Inventory File for KubeMaster
--------------

The function of various variables are explained below as:
 
*master_node_name:* This is needed to create the static file with the hostgroup for master

*slave_nodes_name:* This is needed to create the static file with the hostgroup for slaves

*inventory_dir:* This is needed to specify the folder consisting of inventory

*filter_group:* This is needed to filter out the public IP address of Master node which will eventually get updated into static inventory

Play-3: Making entries to Inventory File for KubeSlaves
---------------

The function of various variables are explained below as:

*master_node_name:* This is needed to create the static file with the hostgroup for master

*slave_nodes_name:* This is needed to create the static file with the hostgroup for slaves

*inventory_dir:* This is needed to specify the folder consisting of inventory

*filter_group:* This is needed to filter out the public IP address of Slave nodes which will eventually get updated into static inventory

*slave_info:* This is where we have to manually update the localhostame and the host ips to be assigned 
Ex--> If we want 4 Slaves then we can have the following code

            - localhostname: myslave1
              hostip: "{{ groups['all'][0] }}"
            - localhostname: myslave2
              hostip: "{{ groups['all'][1] }}"
            - localhostname: myslave3
              hostip: "{{ groups['all'][2] }}"
            - localhostname: myslave4
              hostip: "{{ groups['all'][3] }}"


Play-4: Configuring KubeMaster
-------------

The function of various variables are explained below as:


*master_node_name:* This specifies the hostgroup under which master node is mentioned in the inventory file

*files_path:* This is the location of directory where token-master is to be downloaded


Play-5:  Configuring KubeSlaves
-------------

The function of various variables are explained below as:

*slave_nodes_name:* This specifies the hostgroup under which slave nodes is mentioned in the inventory file

*files_path:* This is the location of directory consisting of token-master

Dependencies
------------

Role1 - [ssjnm.master_slave_ec2] /
Role2 - [ssjnm.kubeadm_aws_master] /
Role3 - [ssjnm.kubeadm_aws_slaves] /


Author Information
------------------

This role was created by Nishant Singh, GitHub User - [SSJNM]


[ssjnm.master_slave_ec2]: <>
[ssjnm.kubeadm_aws_master]: <>
[ssjnm.kubeadm_aws_slaves]: <>
[SSJNM]: <https://github.com/SSJNM>