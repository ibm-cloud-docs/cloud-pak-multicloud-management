---

copyright:
  years: 2020
lastupdated: "2020-05-01"

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

Red Hat Ansible Tower is an Internet-based hub that runs your automation tasks. You can configure the optional license to use Red Hat Ansible Tower with the automation capability of the IBM Cloud Pak® for Multicloud Management.


## Before you begin

- You must have a Linux system with OpenShift command-line tool (oc) installed to run the installer.

- Before you can install Red Hat Ansible Tower, you must download the license key and the `automation-navigation-updates.sh` script from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html). 
  
Download the part numbers CC66KEN and CC5WCEN from IBM Passport Advantage.

| Description                                                                      | File name                               | Passport Advantage part number |
|----------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
| Red Hat Ansible Tower 3.6 key | temporary-tower-license.txt |   CC5WCEN  |
| Automation navigation for IBM Cloud Pak® for Multicloud Management 1.3 | automation-navigation-updates.sh | CC66KEN  |

- For the list of all part numbers, see [Passport Advantage part numbers](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_1.3.0/about/part_numbers.html).
  
- You must have {{site.data.keyword.cp4mcm_full_notm}} installed. For more information, see [Getting started with {{site.data.keyword.cp4mcm_full_notm}}](https://test.cloud.ibm.com/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-getting-started)

- You must have Admin privileges for the account that is used to run the OpenShift installer (`cluster-admin` role is required)

- **Notes**:

   * When you install Red Hat Ansible Tower, your target cluster must be an OpenShift cluster where {{site.data.keyword.cp4mcm_full_notm}} is installed.

   * Create an `ansible-tower` namespace for your Red Hat Ansible installation in your OpenShift cluster.

## Installing Red Hat Ansible Tower
{: #install-ansible}

1. To install Ansible Tower on OpenShift, log in to OpenShift as admin from your Linux system.
   ```
   oc login --token=<token> --server=<URL>
   ```
   Following is an example login command:
   ```
   oc login --token=EtZqGLpwxpL8b6CAjs9Bvx6kxe925a1HlB__AR3gIOs --server=https://c100-e.us-east.containers.cloud.ibm.com:32653
   ```
   You can find the `oc login` for your OpenShift cluster where IBM Cloud Pak for Multicloud Management is installed by using these steps:
   1. Browse and log in to IBM cloud, https://cloud.ibm.com/
   2. Browse to **OpenShift**>**Clusters**
   3. Select your cluster where IBM Cloud Pak for Multicloud Management is installed.
   4. Log in to the OpenShift web console
   5. Click **IAM#<yourID>** menu, then select "Copy Login Command"
   6. Click Display Token link
   7. Copy the provided `oc login ...` CLI command to log in to OpenShift.
    
2. Ensure that Ansible is installed. Run the command:
    ```
    sudo yum install ansible
    ```
    {: codeblock}

3. On your Linux system, download the package for ansible-tower-openshift-setup 

    Run the command:
    ```
    wget https://releases.ansible.com/ansible-tower/setup_openshift/ansible-tower-openshift-setup-latest.tar.gz
    ```
    {: codeblock}

4. Extract the package by running the commands:  
    ```
    tar xvf ansible-tower-openshift-setup-latest.tar.gz
    ```
    {: codeblock}

    ```
    cd ansible-tower-openshift-setup-3.6.4
    ```
    {: codeblock}

5. Create an `ansible-tower` namespace for your Red Hat Ansible installation in your OpenShift cluster.

    ```
    oc new-project ansible-tower
    ```
    {: codeblock}
    ```
    oc project ansible-tower 
    ```
    {: codeblock}
    ```
    oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git
    ```
    {: codeblock}

6. Create a pvc named **postgresql**. Sample PVC resource file named postgres-nfs-pvc.yaml
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
      storageClassName: ibmc-block-gold
    ```
    {: codeblock}

    **Note:** Change the storage class according to your OpenShift Storage Class listing.

    ```
    oc create -f postgres-nfs-pvc.yaml
    ```
    {: codeblock}
    
7. Ensure that the pvc is created.
    ```
    oc get pvc
    ```
    {: codeblock}

8. Modify the Ansible playbook to use insecure login.

    ```
    sed -i "s/{{ openshift_skip_tls_verify | default(false)/{{ openshift_skip_tls_verify | default(true)/g" roles/kubernetes/tasks/openshift_auth.yml
    ```
    {: codeblock}
    
9. Run the installation command:

    ```
    ./setup_openshift.sh -e openshift_host=https://api.green.coc-ibm.com:6443 -e openshift_project=ansible-tower -e openshift_user=admin -e openshift_token=<YOUR_TOKEN> -e admin_password=rudolph-the-reindeer -e secret_key=mysecret -e pg_username=postgresuser -e pg_password=postgrespwd -e rabbitmq_password=rabbitpwd -e rabbitmq_erlang_cookie=rabbiterlangapwd
    ```
    {: codeblock}
    
    **Note:** Modify the values for OpenShift API URL, admin user, admin token, and the passwords you want to set for Ansible Tower.

    This completes the setup of Ansible tower.

10. Log in to Ansible Tower and import the license from the 
`temporary-tower-license.txt` downloaded in "Before you begin".

     When Ansible Tower starts for the first time, the license screen is automatically displayed. Import the license key that you received in `temporary-tower-license.txt`.

11. Click Browse and select the location where the license file is saved to upload it. The uploaded license can be a plain text file or a JSON file, and must include properly formatted JSON code.

    The license is recognized, continue by checking the End-User license Agreement.
    After you specify your tracking and analytics preferences, click Submit.
        
    The license is accepted, and Ansible Tower briefly displays the license screen and takes you to the Dashboard of the Ansible Tower interface. You can access the license screen by clicking the Ansible Tower logo.
 
    Ansible Tower can now be integrated with IBM Cloud Pak for Multicloud Manager.

## Integrate Ansible Tower with IBM Cloud Pak for Multicloud Management

Enable navigation to Red Hat Ansible Tower within the IBM Cloud Pak console.

Complete the following steps on a Linux system. These steps enable navigation to Ansible from the IBM Cloud Pak​​ console:

1. Obtain the Automation navigation for IBM Cloud Pak​​ for Multicloud Management 1.3 script, `automation-navigation-updates.sh`, downloaded from [IBM Passport Advantage®](https://www.ibm.com/software/passportadvantage/) website. This script was downloaded from IBM Passport Advantage in the "Before you begin" section.

2. Install and authenticate `kubectl`. For more information, see [Installing the Kubernetes CLI (kubectl)](https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/kubectl/install_kubectl.html).

3. Install JQ. For more information, see [Download jq](https://stedolan.github.io/jq/download/).

4. Copy the `automation-navigation-updates.sh` script to a directory location. Set the file permissions on the script and run the script to enable navigation to your Ansible instance:

   ```
   chmod 755 ./automation-navigation-updates.sh

   ./automation-navigation-updates.sh -t ansible-tower
   ```

    **Note:** Make sure the `-t <name>` matches the name that is used for the `openshift_project` from Step 9. For example, `./setup_openshift.sh -e openshift_project=ansible-tower`. In this example, `ansible-tower` is the name that is used.
   
5. Verify that the Ansible Tower instance is in the IBM Cloud Pak​​ console navigation menu. From the IBM Cloud Pak​​ navigation menu, click **Automate infrastructure** > **Ansible automation**.

You have now successfully installed Red Hat Ansible Tower and integrated with IBM Cloud Pak console in IBM Cloud.
