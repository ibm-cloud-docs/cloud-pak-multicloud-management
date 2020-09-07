---

copyright:
  years: 2020
lastupdated: "2020-09-07"

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

# Getting started with {{site.data.keyword.cp4mcm_full_notm}}
{: #getting-started}

The {{site.data.keyword.cp4mcm_full_notm}}, running on Red Hat® OpenShift®, is an open, hybrid Cloud Management platform. This platform helps organizations break down IT silos and move operations to start to codify tasks and processes and manage them as code artifacts.

With {{site.data.keyword.cp4mcm_full_notm}}, you get more application and cluster visibility across the enterprise to any public or private cloud. You can improve automation by simplifying your IT and application operations management with increased flexibility and cost savings, and intelligent data analysis driven by predictive signals.

You can also take advantage of the governance with IBM Cloud Pak for Multicloud Management. You can manage your multicloud environments with a consistent and automated set of configuration and security policies across all applications and clusters. For more information, see the [documentation](https://www.ibm.com/support/knowledgecenter/SSFC4F/product_welcome_cloud_pak.html).
{:shortdesc}

## What's inside this Cloud Pak

### Operator-based installation
The {{site.data.keyword.cp4mcm_full_notm}} installation is now operator-based. You can choose either a simple or advanced installation method. When you choose the simple installation, the following default services are installed. 

- Console
- Governance, risk, and compliance
- Global search
- Cluster management
- Application management
- IBM Cloud Platform Common Services
- License Advisor

With the advanced installation mode, you can choose to enable or disable these modules and services:

- Infrastructure management operators, which include Policies and Profiles (ibm-management-infra-grc), Provisioning virtual machines and Instances (ibm-management-infra-vm), Managed services (ibm-management-cam-install), and Creating and managing services (ibm-management-service-library).
- Operations operator, which is ChatOps (ibm-management-sre-chatops).
- Monitoring operator, which is Monitoring (ibm-management-monitoring).
- Security services operators, which include Notary service for image signing (ibm-management-notary), Image signing support for image policies (ibm-management-image-security-enforcement), Mutation Advisor (ibm-management-mutation-advisor), and Vulnerability Advisor (ibm-management-vulnerability-advisor).
- Runtime (ibm-management-manage-runtime) operator, which is a technology preview code.

### Self-service capabilities
Additional self-service capabilities enable developers to provision infrastructure and application components from a predefined service flow. This means providing a full-service library feature for developers to access on-demand.

### SRE tools for hybrid applications
The {{site.data.keyword.cp4mcm_full_notm}} supports the implementation of an SRE operating model. Some of these tools are:

- Bastion control - A new single point of access terminal supports secure remote access to managed systems. This centralized bastion host simplifies the troubleshooting process across regions and deployment models with increased visualization and remediation tools, such as audit and replay.

- Session replay -  Replay sessions for auditing, compliance, and SRE postmortem reviews, which allows SRE teams to have a consistent source of review and feedback for future incident response and resolution.

- ChatOps - A ChatOps interface that connects with your preferred tools, including PagerDuty and Slack. This further accelerates the SRE’s ability to receive and react to incidents immediately using the tools they use every day.

### Hybrid Policy Model

The updates to Governance, Risk, and Compliance policies allow for a faster, more flexible solution for both VMs and containers. This hybrid approach to policy applications allows operators to apply a policy that is enforced, even as applications are edited and moved by development teams. These enhancements also include templated policy-as-code for industry standards such as HIPAA, FISMA, NIST, and PCI. This policy-based Governance and Risk dashboard is ready out of the box and is versatile and adaptable for your unique compliance and governance requirements.

### Cost and license management 

Managing cloud costs is a priority in any organization. {{site.data.keyword.cp4mcm_full_notm}} cost modeling and asset management now provide a more seamless experience. The capabilities that are included are:

- IT Cost Modeling, Showback, and Chargeback are enhanced with new visibility features to find resources and more detailed report generation.

- Chargeback generation steps are simplified to three key steps: Define rate cards, assign rate cards, and generate the chargeback report.

## Before you begin

- Before you can install the Cloud Pak, you must purchase a license through [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html). For part numbers, see [Passport Advantage part numbers](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_1.3.0/about/part_numbers.html).

- You must have a supported version of {{site.data.keyword.openshiftshort}} Container Platform that is installed by using {{site.data.keyword.IBM_notm}} Cloud Kubernetes Service so that the managed {{site.data.keyword.openshiftshort}} service is supported. For the list of supported versions, see [Supported {{site.data.keyword.openshiftshort}} versions and platforms](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_1.3.0/install/supported_os.html).

Question FOR SETH: I presume this is still valid, please confirm.......- You must have the required user and resource group permissions to install the {{site.data.keyword.cp4mcm_full_notm}}:

  - You must have permission as an admin user on the cluster where you are installing the {{site.data.keyword.cp4mcm_full_notm}}.
  - You must have at least viewer access permission to the resource group that includes the cluster where you are installing the {{site.data.keyword.cp4mcm_full_notm}}. Ensure that this resource group exists in IBM Cloud and that the user ID that is used in IBM Cloud Provider cloud connection has access to this resource group.

### Minimum hardware requirements

SETH - FYI, I LEFT THE CPU CORES AT 16 FOR EACH, PLEASE COMMENT IF YOU WANT ME TO CHANGE. HERE: https://www.ibm.com/support/knowledgecenter/SSFC4F_2.0.0/install/hardware_reqs.html THEY ARE ONLY 8 BUT READING THE TEXT ABOVE THE TABLE IN THIS TOPIC IT SAYS....The following table provides the minimum hardware requirements for.....: 

| Master/Worker| Nodes | CPU (Cores/Node) | Memory (GB/Node) |
|----|----|---|----|
| Master | 3 | 16 | 24 |
| Worker | 4+ | 16| 32 |

### Minimum storage requirements

SETH WILL CONFIRM IF ANY CHANGES HERE

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

SETH WILL CONFIRM IF ANY CHANGES TO THIS STEP - SHOULD STAY THE SAME

From the _Create_ tab on the `Cloud Pak for Multicloud Management` installation page, specify where you want to install the {{site.data.keyword.cp4mcm_full_notm}}:

  1. Select the Red Hat {{site.data.keyword.openshiftshort}} cluster where you want to deploy the {{site.data.keyword.cp4mcm_full_notm}}.
  2. Enter or select the project where you want to deploy {{site.data.keyword.cp4mcm_full_notm}}.

## Step 2. Configure your workspace
{: #config-workspace}

SETH WILL CONFIRM IF ANY CHANGES TO THIS STEP - SHOULD STAY THE SAME

Specify how to track and manage your installation from your {{site.data.keyword.IBM_notm}} Cloud Schematics workspace:

  1. In the _Configure your workspace_ section, update the name and tag of the installation workspace.
  2. Specify any tags that you want to use for the installation. Specify multiple tags as a comma-separated list.

## Step 3. Complete the pre-installation check
{: #pre-install-check}

SETH WILL CONFIRM IF ANY CHANGES TO THIS STEP - SHOULD STAY THE SAME APART FROM 3RD BULLET BELOW

A Red Hat {{site.data.keyword.openshiftshort}} cluster administrator must complete this step.

- If you are not an administrator, use the Share link to share the script with your cluster administrator.
- If you are a cluster administrator, click **Run script** to run the pre-installation check. Confirm that the script completes successfully.
- SETH TO CONFIRM IF THIS NEEDS TO BE REMOVED: For any certificate signing requests (CSRs) that are generated on your cluster nodes, approve all of the CSRs on your nodes before you install. Run the following commands on each of your cluster nodes to approve your CSRs:

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

SETH WILL CONFIRM IF ANY CHANGES TO THIS STEP.....MAY NEED TO BE UPDATED SO INSTEAD OF SETTING THESE VALUES WE MAY NEED TO RETRIEVE THEM INSTEAD. THE STEPS TO RETRIEVE THEM ARE ALREADY DOCUMENTED HERE: https://www.ibm.com/support/knowledgecenter/SSFC4F_2.0.0/install/online_install.html (see the **Console username and password** section at the end of this topic)

Override the default values by configuring the required deployment values for the {{site.data.keyword.openshiftshort}} cluster that you installed:

| Parameter | Description | Default |
| -------- | -------- | -------- |
| `defaultAdminUser`  |  Configure default admin username.  | `admin` |
| `defaultAdminPassword`  | Configure default admin user password. Password must be at least 32 characters by default, and can include only number, letter, and hyphens. <p> **Note:** The password that you set during installation might be available as plain text in some pods and logs. You must change the `default_admin_password` after you successfully install {{site.data.keyword.cp4mcm_full_notm}}. For more information about changing the password after {{site.data.keyword.cp4mcm_full_notm}} installation, see [Changing the cluster administrator password](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.0.0/iam/3.4.0/change_admin_passwd.html).</p>|  |
{: caption="Table 1. Deployment values" caption-side="top"}

## Step 5. Install the {{site.data.keyword.cp4mcm_full_notm}}
{: #install-cp4mcm}

SETH WILL CONFIRM IF ANY CHANGES TO THIS STEP - SHOULD STAY THE SAME

1. Ensure that you assigned a license for the {{site.data.keyword.cp4mcm_full_notm}} to the deployment.
2. Confirm that you have read and agree to the license agreements.
3. Click **Install**.

## Post-installation

SETH - TO CHECK IF THIS IS DIFFERENT AND WILL PROVIDE STEPS IF IT IS

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

  1. Log in the {{site.data.keyword.cp4mcm_full_notm}} management console by using the admin username and password that you configured during the installation.
  2. Optional:  After installation, you can choose to enable or disable additional modules and services such Infrastructure Management, Operations, Monitoring, and other services. For instructions, see [Advanced configuration](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.0.0/install/config_adv.html).

## Uninstalling the {{site.data.keyword.cp4mcm_full_notm}}
{: #uninstalling}

To uninstall {{site.data.keyword.cp4mcm_full_notm}}, you can use the {{site.data.keyword.cp4mcm_full_notm}} console or the command-line interface (CLI).

### Uninstalling the {{site.data.keyword.cp4mcm_full_notm}} from the console
SETH - THESE STEPS SHOULD BE THE SAME BUT SETH WILL TEST AND CONFIRM IF ANY CHANGES ARE REQUIRED

1. Enter the workspace of your installed {{site.data.keyword.cp4mcm_full_notm}}.

2. Click the **Actions** button. Then, click the **delete** button to trigger a delete.

3. Choose Delete workspace and Delete all associated resources and input the name of the workspace to confirm.
Click the delete to delete the workspace.

4. Waiting for the uninstall finish.

5. Verify that the Cloud Pak is uninstalled:

   Access the {{site.data.keyword.open_s}} web console and verify that the components that are related to the {{site.data.keyword.cp4mcm_full_notm}}, such as any related pods, are no longer installed.

### Uninstalling the {{site.data.keyword.cp4mcm_full_notm}} on with command line

THE FOLLOWING STEPS WILL BE DIFFERENT - SETH TO TEST AND PROVIDE CHANGES

If the delete operation failed from the console or you want to use command line, you can use the `oc` command-line interface to complete the steps to uninstall. When you use the command line, you need to remove the resources on your {{site.data.keyword.openshiftshort}} Container Platform cluster that are associated with the {{site.data.keyword.cp4mcm_full_notm}}.

If the {{site.data.keyword.openshiftshort}} CLI is not installed, download and install the CLI from the Red Hat Customer Portal.

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
