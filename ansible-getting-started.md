---

copyright:
  years: 2020
lastupdated: "2020-04-22"

keywords: ansible getting started

subcollection: cloud-pak-multicloud-management

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Getting started with Ansible Tower in IBM Cloud
{: #ansible-getting-started}

Red Hat® Ansible Tower automates continuous configurations and deployments.

Red Hat Ansible Tower is an Internet-based hub that runs your automation tasks. You can configure the optional license to use Red Hat Ansible Tower with the automation capability of the IBM Cloud Pak® for Multicloud Management console.


## Before you begin

- You must have a Linux system with OpenShift command-line tool (oc) on the machine running the installer.

- Before you can install Red Hat Ansible Tower, you must download the license key and the `automation-navigation-updates.sh` script from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html). 
  
Download the part numbers CC66KEN and CC5WCEN from IBM Passport Advantage.
| Description                                                                      | File name                               | Passport Advantage part number |
|----------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
| Automation navigation for IBM Cloud Pak for Multicloud Management 1.3 | automation-navigation-updates.sh | CC66KEN  |
| Red Hat Ansible Tower 3.6 key | temporary-tower-license.txt | CC5WCEN |

- For the list of all part numbers, see [Passport Advantage part numbers](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_1.3.0/about/part_numbers.html).
  
- You must have {{site.data.keyword.cp4mcm_full_notm}} installed. For more information, see [Getting started with {{site.data.keyword.cp4mcm_full_notm}}](https://test.cloud.ibm.com/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-getting-started)
- You must have Admin privileges for the account running the OpenShift installer (`cluster-admin` role is required)
- **Notes**:

   * When you install Red Hat Ansible Tower, your target cluster must be an OpenShift cluster where {{site.data.keyword.cp4mcm_full_notm}} is installed.

   * Create an `ansible-tower` namespace for your Red Hat Ansible installation in your OpenShift cluster.

## Installing Red Hat Ansible Tower
{: #install-ansible}

To install Ansible Tower on OpenShift, log in to OpenShift as admin from your Linux system.
Ensure that ansible is installed.
To install ansible on this

```
apt -y install ansible
```

On your jump server / bastion server download the package for ansible-tower-openshift-setup
**Note:** Latest package is 3.6.3 version ( as of 4/3/2020 )

```
wget https://releases.ansible.com/ansible-tower/setup_openshift/ansible-tower-openshift-setup-latest.tar.gz
```
```
tar xvf ansible-tower-openshift-setup-latest.tar.gz
```
```
cd ansible-tower-openshift-setup-3.6.3
```
Log in to the OpenShift CLI

```
oc login -u <user-id> -p <password> <API URL>

oc new-project tower
oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git
```

Create a pvc named **postgresql**. Sample PVC resource file named postgres-nfs-pvc.yaml

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  annotations:
  labels:
  name: postgresql
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: managed-nfs-storage
```
**Note:** Change the storage class as per your environment

```
oc create -f postgres-nfs-pvc.yaml
```

Ensure that the pvc is created

```
oc get pvc
```

Modify the ansible playbook to use insecure login (I didn't pursue to investigate to use via CLI)

```
sed -i "s/{{ openshift_skip_tls_verify | default(false)/{{ openshift_skip_tls_verify | default(true)/g" roles/kubernetes/tasks/openshift_auth.yml
```

Now you are ready to run the installation

```
./setup_openshift.sh -e openshift_host=https://api.green.coc-ibm.com:6443 -e openshift_project=tower -e openshift_user=admin -e openshift_password=rudolph-the-reindeer -e admin_password=rudolph-the-reindeer -e secret_key=mysecret -e pg_username=postgresuser -e pg_password=postgrespwd -e rabbitmq_password=rabbitpwd -e rabbitmq_erlang_cookie=rabbiterlangapwd
```
**Note:** Modify the OpenShift API URL, admin user, admin password, and the passwords you want to set for Ansible Tower.

This will completed the setup of ansible tower. Once you log in to the console, you must apply the license. This can then be integrated with IBM Cloud Pak for Multicloud Manager.

## Integrate Ansible Tower with IBM Cloud Pak for Multicloud Management

Download the part number CC66KEN from 
| Description                                                                      | File name                               | Passport Advantage part number |
|----------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
| Automation navigation for IBM Cloud Pak for Multicloud Management 1.3 | automation-navigation-updates.sh | CC66KEN  |
| Red Hat Ansible Tower 3.6 key | temporary-tower-license.txt | CC5WCEN |




Enable navigation to Red Hat Ansible Tower within the IBM Cloud Pak console.

Complete the following steps on a Linux system. These steps enable navigation to Ansible from the IBM Cloud Pak​​ console:

1. Obtain the Automation navigation for IBM Cloud Pak​​ for Multicloud Management 1.3 script, `automation-navigation-updates.sh`, from [IBM Passport Advantage®](https://www-01.ibm.com/software/passportadvantage/) website. This script was downloaded from IBM Passport Advantage in the "Before you begin" section.

2. Install and authenticate `kubectl`. For more information, see [Installing the Kubernetes CLI (kubectl)](https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/kubectl/install_kubectl.html).

3. Install JQ. For more information, see [Download jq](https://stedolan.github.io/jq/download/).

4. Copy the `automation-navigation-updates.sh` script to a directory location. Set the file permissions on the script and run the script to enable navigation to your Ansible instance:

   ```
   chmod 755 ./automation-navigation-updates.sh

   ./automation-navigation-updates.sh -t tower
   ```
   

5. Verify that the Ansible Tower instance is in the IBM Cloud Pak​​ console navigation menu. From the IBM Cloud Pak​​ navigation menu, click **Automate infrastructure** > **Ansible automation**.

Red Hat Ansible Tower is integrated with the IBM Cloud Pak​​ console.
