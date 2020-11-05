---

copyright:
  years: 2020
lastupdated: "2020-04-15"

keywords: getting started tutorial, getting started, cloud-pak-multicloud_management

subcollection: cloud-pak-multicloud-management

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Getting started with {{site.data.keyword.cp4mcm_full_notm}} V1.3
{: #getting-started}

The {{site.data.keyword.cp4mcm_full_notm}}, running on Red Hat OpenShift, provides consistent visibility, governance, and automation from on-premises to the edge. Enterprises gain capabilities such as multicluster management, event management, application management, and infrastructure management. Enterprises can use this Cloud Pak to help increase operational efficiency that is driven by intelligent data, analysis, and predictive golden signals, and gain built-in support for their compliance management. For more details, see the [documentation](https://www.ibm.com/support/knowledgecenter/SSFC4F/product_welcome_cloud_pak.html).
{:shortdesc}

## What's inside this Cloud Pak

In addition to the default features for managing multicloud environments, the IBM Cloud Pak for Multicloud Management includes the following installable modules that you can add to your cluster to manage applications and infrastructure, and to automate tasks:

- Monitoring Module for monitoring the performance and availability of cloud applications in hybrid cloud environments.
- Terraform & Service Automation Module for cluster security, operating efficiency, and appropriate service level delivery.
- CloudForms for controlling and managing cloud infrastructures.
- Red Hat Ansible Tower for running your automation tasks.

## Before you begin

- Before you can install the Cloud Pak, you must purchase a license through [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html). For part numbers, see [Passport Advantage part numbers](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_1.3.0/about/part_numbers.html).

- You must have a supported version of {{site.data.keyword.openshiftshort}} Container Platform installed by using {{site.data.keyword.IBM_notm}} Cloud Kubernetes Service so that the managed {{site.data.keyword.openshiftshort}} service is supported. For the list of supported versions, see [Supported {{site.data.keyword.openshiftshort}} versions and platforms](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_1.3.0/install/supported_os.html).

- You must have a pre-configured StorageClass in {{site.data.keyword.openshiftshort}} that can be used for installing the {{site.data.keyword.cp4mcm_full_notm}}.

- You must have the required user and resource group permissions to install the {{site.data.keyword.cp4mcm_full_notm}}:

  - You must have permission as an admin user on the cluster where you are installing the {{site.data.keyword.cp4mcm_full_notm}}.
  - You must have at least viewer access permission to the resource group that includes the cluster where you are installing the {{site.data.keyword.cp4mcm_full_notm}}. Ensure that this resource group exists in IBM Cloud and that the user ID that is used in IBM Cloud Provider cloud connection has access to this resource group.

### Minimum hardware requirements

| Nodes | Memory | CPU |
|----|---|----|
| 3 | 32 GB | 16 cores |

**Notes:** If you are going to install the Monitoring Module and Terraform & Service Automation Module with the {{site.data.keyword.cp4mcm_full_notm}}, you need another two nodes.

### Minimum storage requirements

Mandatory: 2 PV

| Component | PV |
|-----|-----|
| MongoDB | 1 |
| {{site.data.keyword.IBM_notm}} Multicloud Manager etcd | 1 |

Optional: 4 PV

| Component | PV |
|------|------|
| Logging | 1 |
| VA | 3 |

## Step 1. Configure your installation environment
{: #config-enviro}

From the _Create_ tab on the `Cloud Pak for Multicloud Management` installation page, specify where you want to install the {{site.data.keyword.cp4mcm_full_notm}}:

  1. Select the Red Hat {{site.data.keyword.openshiftshort}} cluster where you want to deploy the {{site.data.keyword.cp4mcm_full_notm}}.
  2. Enter or select the project where you want to deploy {{site.data.keyword.cp4mcm_full_notm}}.

## Step 2. Configure your workspace
{: #config-workspace}

Specify how to track and manage your installation from your {{site.data.keyword.IBM_notm}} Cloud Schematics workspace:

  1. In the _Configure your workspace_ section, update the name and tag of the installation workspace.
  2. Specify any tags that you want to use for the installation. Specify multiple tags as a comma-separated list.

## Step 3. Complete the pre-installation check
{: #pre-install-check}

A Red Hat {{site.data.keyword.openshiftshort}} cluster administrator must complete this step.

- If you are not an administrator, use the Share link to share the script with your cluster administrator.
- If you are a cluster administrator, click **Run script** to run the pre-installation check. Confirm that the script completes successfully.
- For any certificate signing requests (CSRs) that are generated on your cluster nodes, approve all of the CSRs on your nodes before you install. Run the following commands on each of your cluster nodes to approve your CSRs:

   1. Find all CSRs for your cluster nodes:

      ```
      oc get csr
      ```
      {: pre}

   2. Approve the CSRs for the cluster nodes:

      ```
      oc adm certificate approve <CSR name>
      ```
      {: pre}

   For more information, see the [Approving the CSRs for your machines](https://docs.openshift.com/container-platform/4.2/machine_management/more-rhel-compute.html#installation-approve-csrs_more-rhel-compute) topic in the OpenShift documentation.

## Step 4. Set the deployment values
{: #set-deploy-values}

Override the default values by configuring the required deployment values for the {{site.data.keyword.openshiftshort}} cluster that you have installed:

| Parameter | Description | Default |
| -------- | -------- | -------- |
| `defaultAdminUser`  |  Configure default admin user name.  | `admin` |
| `defaultAdminPassword`  | Configure default admin user password. Password must be at least 32 characters by default, and can include only number, letter, and hyphens. <p> **Note:** The password that you set during installation might be available as plain text in some pods and logs. You must change the `default_admin_password` after you successfully install {{site.data.keyword.cp4mcm_full_notm}}. For more information about changing the password after {{site.data.keyword.cp4mcm_full_notm}} installation, see [Changing the cluster administrator password](https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/iam/3.4.0/change_admin_passwd.html).</p>|  |
| `storageClass`  |  Configure storage class. | `ibmc-block-bronze` |
{: caption="Table 1. Deployment values" caption-side="top"}

## Step 5. Install the {{site.data.keyword.cp4mcm_full_notm}}
{: #install-cp4mcm}

1. Ensure that you have assigned a license for the {{site.data.keyword.cp4mcm_full_notm}} to the deployment.
2. Confirm that you have read and agree to the license agreements.
3. Click **Install**.

## Post-installation

To interact with the {{site.data.keyword.cp4mcm_full_notm}} by using the management console from your local machine, perform the following steps:

1. Run `kubectl cluster-info` to get the Kubernetes API server address and port. Example output:
   ```
   kubectl cluster-info
   Kubernetes master is running at https://honest-gryphon-master.purple-chesterfield.com:7443
   ```
   {: pre}

   **Note**: From the `kubectl cluster-info` output, make a note of the host and port for the Kubernetes master. In the example, the host is `honest-gryphon-master.purple-chesterfield.com` and the port is `7443`.

2. Run `kubectl edit cm ibmcloud-cluster-info -n kube-public`. Example output:
   ```
   apiVersion: v1
   data:
     cluster_address: icp-console.chee-ocp-fae6c235d7a3aed6346697c0e75f4896-0001.us-east.containers.appdomain.cloud
     cluster_ca_domain: icp-console.chee-ocp-fae6c235d7a3aed6346697c0e75f4896-0001.us-east.containers.appdomain.cloud
     cluster_endpoint: https://icp-management-ingress.kube-system.svc:443
     cluster_kube_apiserver_host: honest-gryphon-master.purple-chesterfield.com
     cluster_kube_apiserver_port: "7443"
     cluster_name: mycluster
     cluster_router_http_port: "8080"
     cluster_router_https_port: "443"
     edition: Enterprise Edition
     openshift_router_base_domain: chee-ocp-fae6c235d7a3aed6346697c0e75f4896-0001.us-east.containers.appdomain.cloud
     proxy_address: icp-proxy.chee-ocp-fae6c235d7a3aed6346697c0e75f4896-0001.us-east.containers.appdomain.cloud
     proxy_ingress_http_port: "80"
     proxy_ingress_https_port: "443"
     version: 1.2.0
   ```
   {: pre}

3. Verify the `cluster_kube_apiserver_host` and `cluster_kube_apiserver_port` are correctly set with the host and port values from step one for the Kubernetes master and then save the file.

## Next steps

When the installation completes, you can access your {{site.data.keyword.cp4mcm_full_notm}} deployment with the provided URL.

  1. Log in the {{site.data.keyword.cp4mcm_full_notm}} management console by using the admin user name and password that you configured during the installation.
  2. Optional: Install any optional modules for the {{site.data.keyword.cp4mcm_full_notm}}. For instructions, see [Installing, configuring, and upgrading the modules](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_1.3.0/install/install_modules.html).

## Uninstalling the {{site.data.keyword.cp4mcm_full_notm}}
{: #uninstalling}

To uninstall {{site.data.keyword.cp4mcm_full_notm}}, you can use the {{site.data.keyword.cp4mcm_full_notm}} console or the command-line interface (CLI).

### Uninstalling the {{site.data.keyword.cp4mcm_full_notm}} from the console

1. Enter the workspace of your installed {{site.data.keyword.cp4mcm_full_notm}}.

2. Click the Actions button in the upper right corner. Then, click the delete button to trigger a delete.

3. Choose Delete workspace and Delete all associated resources and input the name of the workspace to confirm.
Click the delete to delete the workspace.

4. Waiting for the uninstall finish.

5. Verify that the Cloud Pak is uninstalled:

   Access the {{site.data.keyword.open_s}} web console and verify that the components that are related to the {{site.data.keyword.cp4mcm_full_notm}}, such as any related pods, are no longer installed.

### Uninstalling the {{site.data.keyword.cp4mcm_full_notm}} on with command-line

If the delete operation failed from the console or you want to use command-line, you can use the `oc` command-line interface to complete the steps to uninstall. When you use the command-line, you need to remove the resources on your {{site.data.keyword.openshiftshort}} Container Platform cluster that are associated with the {{site.data.keyword.cp4mcm_full_notm}}.

If you do not have the {{site.data.keyword.openshiftshort}} CLI installed, download and install the CLI from the Red Hat Customer Portal.

When you are running the commands to remove the associated resources, use the project that you selected during the installation of your OpenShift Container Platform cluster. Replace the `<project_name>` parameter in the following commands with your project name.

1. Delete the resources for the custom resource definition (CRD):
    ```
    oc -n <project_name> delete IBMServicesPlatform default
    ```
    {: pre}

2. Delete the `deployment`:
    ```
    oc -n <project_name> delete deployment ibmservices-operator
    ```
    {: pre}

3. Delete the custom resource definition:
    ```
    oc -n <project_name> delete CustomResourceDefinition IbmServicesPlatforms.operator.ibm.com
    ```
    {: pre}

4. Delete the `ClusterRoleBinding`:
    ```
    oc -n <project_name> delete clusterrolebinding ibmservices-operator
    ```
    {: pre}

5. Delete the pull `secret`:
    ```
    oc -n <project_name> delete secret ibmplatform-image-pull-secret
    ```
    {: pre}

6. Delete the `admin-credential`:
    ```
    oc -n <project_name> delete secret admin-credential
    ```
    {: pre}

7. Delete configmap `cloudpak-foundation`:
    ```
    oc -n kube-system delete configmap cloudpak-foundation
    ```
    {: pre}

8. Verify that the Cloud Pack is uninstalled.

    Access the {{site.data.keyword.openshiftshort}} web console and verify that the components that are related to the {{site.data.keyword.cp4mcm_full_notm}}, such as any related pods, are no longer installed.
