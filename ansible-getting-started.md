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
- Your target cluster must be an OpenShift cluster where IBM Cloud Pak for Multicloud Management is installed. For more information, see [Getting started with IBM Cloud Pak for Multicloud Management](https://cloud.ibm.com/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-getting-started).
- You must have administrator privileges for the account that is used to run the OpenShift installer (`cluster-admin` role is required).
- Download the  IBM Cloud Pak® for Multicloud Management 2.1 part numbers from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html) (earlier versions are also available). 

|Version|Description|Filename|Passport Advantage number|
|-----|-----|-----|-----|
|IBM Cloud Pak® for Multicloud Management 2.1|Red Hat Ansible Tower key|temporary-tower-license.txt|CC737EN|
|IBM Cloud Pak® for Multicloud Management 2.1|Automation navigation for IBM Cloud Pak® for Multicloud Management 2.1|automation-navigation-updates.sh|CC734EN|
|IBM Cloud Pak® for Multicloud Management 2.0|Red Hat Ansible Tower key|temporary-tower-license.txt|CC737EN|
|IBM Cloud Pak® for Multicloud Management 2.0|Automation navigation for IBM Cloud Pak for Multicloud Management 2.0|automation-navigation-updates.sh|CC734EN|
|IBM Cloud Pak® for Multicloud Management 1.3|Red Hat Ansible Tower key|temporary-tower-license.txt|CC7X6EN|
|IBM Cloud Pak® for Multicloud Management 1.3|Automation navigation for IBM Cloud Pak for Multicloud Management 1.3 |automation-navigation-updates.sh|CC737EN|


## Installing Red Hat Ansible Tower
{: #install-ansible}

1. Log in to OpenShift as an administrator from your Linux system.
   ```
   oc login --token=<token> --server=<URL>
   ```
   For example:
   ```
   oc login --token=EtZqGLpwxpL8b6CAjs9Bvx6kxe925a1HlB__AR3gIOs --server=https://c100-e.us-east.containers.cloud.ibm.com:32653
   ```
   You can find the `oc login` for your OpenShift cluster where IBM Cloud Pak for Multicloud Management is installed by using these steps:

      a. Log in to IBM Cloud: https://cloud.ibm.com/  
      b. Select **OpenShift**>**Clusters** and select the cluster where IBM Cloud Pak for Multicloud Management is installed.  
      c. Log in to the OpenShift web console and select the  **IAM &lt;yourID&gt;** menu, then select "Copy Login Command".  
      d. Click the Display Token link.  
      e. Copy the provided `oc login ...` CLI command to log in to OpenShift.  
    
2. On your Linux system, download the installation package by running the following command: 

    ```
    wget https://releases.ansible.com/ansible-tower/setup_openshift/ansible-tower-openshift-setup-latest.tar.gz
    ```
    {: codeblock}

3. Extract the package by running the following commands:  
    ```
    tar xvf ansible-tower-openshift-setup-latest.tar.gz
    ```
    {: codeblock}

    ```
    cd ansible-tower-openshift-<version_from_step2>
    ```
    {: codeblock}

4. To install Ansible, run the following command:
    ```
    sudo yum install ansible
    ```
    {: codeblock}
    Note: Ensure the ansible version installed is the required version for the setup bundle you downloaded, see the [Ansible Software Requirements ![Opens in a new tab](../images/icons/launch-glyph.svg "Opens in a new tab")](https://docs.ansible.com/ansible-tower/3.7.1/html/installandreference/requirements_refguide.html#ansible-software-requirements) in the Ansible Tower documentation.  
    Run this command to check ansible version: ``<ansible --version>``.

5. Create an `ansible-tower` namespace for your Red Hat Ansible installation in your OpenShift cluster. 

    ```
    oc new-project ansible-tower
    ```
    {: codeblock}

6. (Optional) Switch to the new project/namespace and add an application: 
    ```
    oc project ansible-tower 
    ```
    {: codeblock}
    ```
    oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git
    ```
    {: codeblock}

7. Create a pvc named **postgresql**. Sample PVC resource file named postgres-nfs-pvc.yaml
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
    
8. Ensure that the pvc is created.
    ```
    oc get pvc
    ```
    {: codeblock}

9. Modify the Ansible playbook to use insecure login.

    ```
    sed -i "s/{{ openshift_skip_tls_verify | default(false)/{{ openshift_skip_tls_verify | default(true)/g" roles/kubernetes/tasks/openshift_auth.yml
    ```
    {: codeblock}
    
10. Run the installation command to complete the setup:

      ```
    ./setup_openshift.sh -e openshift_host=https://<host>:<port> -e openshift_project=ansible-tower -e openshift_user=<openshift_user> -e openshift_token=<openshift-token>  
     -e admin_password=<admin-pass> -e secret_key=<my-secret> -e pg_username=<pg_username> -e pg_password=<pg_password> -e rabbitmq_password=<rabbitmq-pass> -e 
    rabbitmq_erlang_cookie=<rabbiterlangapwd>
    ```
    {: codeblock}

    - Modify the values for the following parameters: `openshift_host, openshift_user, openshift_token, admin_password, secret_key, pg_username, pg_password, rabbitmq_password, and rabbitmq_erlang_cookie`.  
     Where  
    - ``<host>`` is the cluster_kube_apiserver_host, for example ``https://api.green.coc-ibm.com:6443 ``.
    - ``<port>`` is the cluster_kube_apiserver_port. 
    - To find the ``<host>`` and ``<port>`` variables, run this command: `kubectl get configmap -n kube-public -o yaml`.
    - ``<openshift_user>`` is the OpenShift admin user.
    - ``<openshift-token>`` is the token that you obtain by running this command ``oc whoami -t`` or login to the OpenShift console and click the user's Copy Login Command.
    - ``<admin-pass>`` is the password that you want to set for Ansible Tower admin user.
    - ``<my-secret>`` is the secret that you want to set. It can be a base64 encoded string.
    - ``<pg_username>`` is the username for postgresql.
    - ``<pg_password``> is the password for postgresql.
    - ``<rabbitmq_pass>`` is the password for Rabbit MQ.
    - ``<rabbiterlangapwd>`` is the Rabbit MQ erlang cookie.  

11. Log in to Ansible Tower and import the license from the ``temporary-tower-license.txt`` downloaded in *Before you begin* section. The first time you log in to Ansible Tower, the license screen is automatically displayed:
	```
    http://<ansible-tower-host>
    ```

    Run this command to locate the <ansible-tower-host> value:
    ```
    oc get route -n ansible-tower
    ```
    {: codeblock}
    The default admin-username is ``admin`` and password is the password you set for the ``admin_password`` parameter during installation, in the ``setup_openshift.sh`` command.


## Integrate Ansible Tower with IBM Cloud Pak for Multicloud Management

Enable navigation to Red Hat Ansible Tower within the IBM Cloud Pak console.

Complete the following steps on a Linux system. These steps enable navigation to Ansible from the IBM Cloud Pak​​ console:

1. Obtain the menu customization script, `automation-navigation-updates.sh`, from [IBM Passport Advantage® ![Opens in a new tab](../images/icons/launch-glyph.svg "Opens in a new tab")](https://www-01.ibm.com/software/passportadvantage/){: new_window} website. You must run the script on a Linux operating system.

2. Install and authenticate `kubectl`. For more information, see [Installing the Kubernetes CLI (kubectl)![Opens in a new tab](../images/icons/launch-glyph.svg "Opens in a new tab")]((https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/kubectl/install_kubectl.html)).

3. Download and configure the JQ tool by using the following commands:
   ```
   wget -O jq https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64

   chmod +x ./jq

   mv jq /usr/bin
   ```
   For more information, see [Download jq ![Opens in a new tab](../images/icons/launch-glyph.svg "Opens in a new tab")](https://stedolan.github.io/jq/download/){: new_window}.

4. Copy the `automation-navigation-updates.sh` script to a directory location. Set the file permissions on the script and run the script to enable navigation to your Ansible instance:

   ```
   chmod 755 ./automation-navigation-updates.sh

   ./automation-navigation-updates.sh -a <name>
   ```

    where &lt;name&gt; is the name that is used for the `openshift_project` paramater in step 10.   
    For example, `./setup_openshift.sh -e openshift_project=ansible-tower`. In this example, `ansible-tower` is the name that is used.
   
5. Verify that the Ansible Tower instance is in the IBM Cloud Pak​​ console navigation menu. From the IBM Cloud Pak​​ navigation menu, click **Automate infrastructure** > **Ansible automation**.

You have now successfully installed Red Hat Ansible Tower and integrated with IBM Cloud Pak console in IBM Cloud.
