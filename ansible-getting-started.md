---

copyright:
  years: 2020
lastupdated: "2020-04-14"

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

To install ansible tower on openshift, log in to OpenShift as admin from your jump server, bastion node etc. Ensure ansible is installed.
For example, the jump server where this was tested is based on ubuntu 18.04 LTS. To install ansible on this

```
apt -y install ansible
```

On your jump server / bastion server download the package for ansible-tower-openshift-setup
**Note: ** Latest package is 3.6.3 version ( as of 4/3/2020 )

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

Ensure the pvc is created

```
oc get pvc
```

Modify the ansible playbook to use insecure login ( I didn't pursue to investigate to use via CLI)

```
sed -i "s/{{ openshift_skip_tls_verify | default(false)/{{ openshift_skip_tls_verify | default(true)/g" roles/kubernetes/tasks/openshift_auth.yml
```

Now you are ready to run the install

```
./setup_openshift.sh -e openshift_host=https://api.green.coc-ibm.com:6443 -e openshift_project=tower -e openshift_user=admin -e openshift_password=rudolph-the-reindeer -e admin_password=rudolph-the-reindeer -e secret_key=mysecret -e pg_username=postgresuser -e pg_password=postgrespwd -e rabbitmq_password=rabbitpwd -e rabbitmq_erlang_cookie=rabbiterlangapwd
```
**Note:** Modify the OpenShift API URL, admin user, admin password and the passwords you want to set for ansible-tower.

This will completed the setup of ansible tower. Once you login to the console, you have to apply the license. This can then be integrated with MCM.

## Integrate with MCM

Download the package
https://na.artifactory.swg-devops.com/artifactory/webapp/#/artifacts/browse/tree/General/orpheus-local-generic/cloudforms/pre-release/automation-navigation-updates.sh

```
chmod 755 automation-navigation-updates.sh
./automation-navigation-updates.sh -t tower 
```
