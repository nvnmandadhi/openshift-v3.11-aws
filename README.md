### openshift-v3.11-aws

#### Description:
     This repository helps you provison an Openshift(OKD) v3.11 cluster using Ansible on AWS

#### Instructions:
     1. Use the template file to create a stack in AWS
     2. Once the stack is created, establish passwordless ssh between the machines
     3. Edit the inventoty.ini and replace the hostnames from the newly created instances
     4. Make sure to use the public and private hostnames
     3. SSH into master instance and run the prepare playbook
     5. Install the following on master instance
        sudo yum install -y git httpd-tools
        sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
        sudo sed -i -e "s/^enabled=1/enabled=0/" /etc/yum.repos.d/epel.repo
        sudo yum -y --enablerepo=epel install ansible pyOpenSSL
     6. Clone the ansible repository to provision the cluster
        git clone https://github.com/openshift/openshift-ansible
        cd openshift-ansible
        git checkout release-3.11
     7. Run the installation playbooks
        cd openshift-ansible
        ansible-playbook [-i /path/to/inventory] playbooks/prerequisites.yml
        ansible-playbook [-i /path/to/inventory] playbooks/deploy_cluster.yml
     8. Wait for the installation process to complete
     9. Create the admin user and assign cluster-admin role
        sudo htpasswd -c /etc/orign/master/htpasswd admin
        oc adm policy add-cluster-role-to-user cluster-admin admin
     10. The cluster can be accessed on https://console.apps.<public_dns_master>
