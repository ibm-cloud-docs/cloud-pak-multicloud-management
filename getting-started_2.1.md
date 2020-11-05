---

copyright:
  years: 2020
lastupdated: "2020-11-05"

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

# Getting started with {{site.data.keyword.cp4mcm_full_notm}} V2.1
{: #getting-started}

The IBM Cloud Pak® for Multicloud Management, running on Red Hat® OpenShift®, is an open, hybrid Cloud Management platform. This platform helps organizations break down IT silos and move operations to start to codify tasks and processes and manage them as code artifacts.

With this IBM Cloud Pak®, you get more application and cluster visibility across the enterprise to any public or private cloud. You can improve automation by simplifying your IT and application operations management with increased flexibility and cost savings, and intelligent data analysis driven by predictive signals.

You can also take advantage of the governance with the {{site.data.keyword.cp4mcm_full_notm}}. You can manage your multicloud environments with a consistent and automated set of configuration and security policies across all applications and clusters. For more information, go to the [documentation](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/kc_welcome_cloud_pak.html).
{:shortdesc}

## What's inside this IBM Cloud Pak

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

## Install {{site.data.keyword.cp4mcm_full_notm}}

### Before you begin

- Before you can install the Cloud Pak, you must purchase a license through [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html). For part numbers, see [Passport Advantage part numbers](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_2.1.0/about/part_numbers.html).

- You must have a supported version of {{site.data.keyword.openshiftshort}} Container Platform that is installed by using {{site.data.keyword.IBM_notm}} Cloud Kubernetes Service so that the managed {{site.data.keyword.openshiftshort}} service is supported. For the list of supported versions, see [Supported {{site.data.keyword.openshiftshort}} versions and platforms](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_2.1.0/install/supported_os.html).

  - You must have permission as an administrator user on the cluster where you are installing the {{site.data.keyword.cp4mcm_full_notm}}.
  - You must have at least viewer access permission to the resource group that includes the cluster where you are installing  {{site.data.keyword.cp4mcm_full_notm}}. Ensure that this resource group exists in IBM Cloud and that the user ID that is used in the IBM Cloud Provider cloud connection has access to this resource group.

### Supported upgrade paths

{{site.data.keyword.cp4mcm_full_notm}} supports upgrading within the the main version. It does not support upgrading between main versions. For example, you cannot upgrade from Version 1.x to Version 2.x. The supported {{site.data.keyword.cp4mcm_full_notm}} upgrade paths within the main version are:

- {{site.data.keyword.cp4mcm_full_notm}} Version 1.2.0 to Version 1.3.0
- {{site.data.keyword.cp4mcm_full_notm}} Version 1.3.0 to Version 1.3.1, 1.3.2
- {{site.data.keyword.cp4mcm_full_notm}} Version 1.3.1 to Version 1.3.2
- {{site.data.keyword.cp4mcm_full_notm}} Version 2.1.0 to Version 2.1.5

### Minimum hardware requirements

| Worker| Nodes | CPU (Cores/Node) | Memory (GB/Node) |
|----|----|---|----|
| Worker | 4+ | 16| 32 |

### Minimum storage requirements

Mandatory: 240GB of persistent storage.

### Procedure

#### Step 1. Configure your installation environment
{: #config-enviro}

From the _Create_ tab on the **Cloud Pak for Multicloud Management** installation page in the IBM Catalog on IBM Cloud, specify where you want to install {{site.data.keyword.cp4mcm_full_notm}}:

  1. Select the Red Hat {{site.data.keyword.openshiftshort}} cluster where you want to deploy {{site.data.keyword.cp4mcm_full_notm}}.
  2. Enter or select the project where you want to deploy {{site.data.keyword.cp4mcm_full_notm}}.

#### Step 2. Configure your workspace
{: #config-workspace}

Specify how to track and manage your installation from your {{site.data.keyword.IBM_notm}} Cloud Schematics workspace:

  1. In the **Configure your workspace** section, update the name and tag of the installation workspace.
  2. Specify any tags that you want to use for the installation. Specify multiple tags as a comma-separated list.

#### Step 3. Complete the pre-installation check
{: #pre-install-check}

A Red Hat {{site.data.keyword.openshiftshort}} cluster administrator must complete this step.

- If you are not an administrator, use the Share link to share the script with your cluster administrator.
- If you are a cluster administrator, click **Run script** to run the pre-installation check. Confirm that the script completes successfully.

#### Step 4. Install {{site.data.keyword.cp4mcm_full_notm}}
{: #install-cp4mcm}

1. Ensure that you assigned a license for {{site.data.keyword.cp4mcm_full_notm}} to the deployment.
2. Confirm that you read and agree to the license agreements.
3. Click **Install**.

### Next steps

When the installation completes, you can access your {{site.data.keyword.cp4mcm_full_notm}} deployment with the provided URL.

1. Log in the {{site.data.keyword.cp4mcm_full_notm}} management console by using the administrator username and password which were generated when the workspace was created.

  - To get the administrator username, run `oc get secret platform-auth-idp-credentials -n ibm-common-services -o jsonpath='{.data.admin_username}' | base64 -d; echo ""`

  - To get the administrator password, run `oc get secret platform-auth-idp-credentials -n ibm-common-services -o jsonpath='{.data.admin_password}' | base64 -d; echo ""`

2. For more information about changing the password after {{site.data.keyword.cp4mcm_full_notm}} is installed, see [Changing the cluster administrator password](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/iam/3.4.0/change_admin_passwd.html).</p>

3. Optional:  After installation, you can choose to enable or disable additional operators and services such Infrastructure Management, Operations, Monitoring, and other services. For instructions, refer to [Advanced configuration](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/install/config_adv.html#edit).

## Uninstall {{site.data.keyword.cp4mcm_full_notm}}
{: #uninstalling}

To uninstall {{site.data.keyword.cp4mcm_full_notm}}, you can use the {{site.data.keyword.cp4mcm_full_notm}} console or the command-line interface (CLI).

### Uninstalling {{site.data.keyword.cp4mcm_full_notm}} from the IBM Cloud console

1. Access https://cloud.ibm.com/schematics/workspaces to go to the schematic workspaces for your {{site.data.keyword.cp4mcm_full_notm}} installation on the IBM Cloud console.

2. Select the workspace where {{site.data.keyword.cp4mcm_full_notm}} is installed, click the button at the end of the row and click **Delete**.

3. Select the **Delete workspace** and **Delete all associated resources** check boxes and enter the name of the workspace in the field. The workspace name is provided for you, just copy or type it in. Click **Delete** to delete the workspace.

4. Wait for the uninstall to finish.

5. Verify that the Cloud Pak is uninstalled by accessing the {{site.data.keyword.openshiftshort}} web console and verifying that the {{site.data.keyword.cp4mcm_full_notm}} components such as the pods and resources are no longer available.

### Uninstalling {{site.data.keyword.cp4mcm_full_notm}} using the command line

If the delete operation failed from the console or you want to use command line, you can use the `oc` command-line interface to complete the steps to uninstall. When you use the command line, you need to remove the resources on your {{site.data.keyword.openshiftshort}} Container Platform cluster that are associated with {{site.data.keyword.cp4mcm_full_notm}}.

If the {{site.data.keyword.openshiftshort}} CLI is not installed, download and install the CLI from the Red Hat Customer Portal.

When you are running the commands to remove the associated resources, use the project that you selected during the installation of your OpenShift Container Platform cluster. Replace the `<project_name>` parameter in the following commands with your project name.

1. Download the `uninstall.sh` script from [github](https://github.com/IBM/cp4mcm-samples/blob/master/scripts/uninstall.sh). Run:
  ```
  curl https://raw.githubusercontent.com/IBM/cp4mcm-samples/master/scripts/cp4mcm-cleanup-utility.sh -o uninstall.sh
  ```
  {: codeblock}

2. Identify the path to the kubeconfig file for your cluster, for example: `/root/.kube/config`
3. Change the file permissions on the `uninstall.sh` script so you can run it:
  ```
  chmod 700 uninstall.sh
  ```
  {: codeblock}

4. Run the `uninstall.sh` script as follows:
  ```
  ./uninstall.sh --mode uninstallEverything --kubeconfigPath ~/.kube/config --cloudpakNamespace <project_name>
  ```
5. Verify that {{site.data.keyword.cp4mcm_full_notm}} is uninstalled by accessing the {{site.data.keyword.openshiftshort}} web console and verifying that the {{site.data.keyword.cp4mcm_full_notm}} components such as the pods and resources are no longer available.

## Troubleshooting

### 1. Mutation Advisor minio pod is not working correctly

#### Problem

When the Mutation Advisor (`ibm-management-mutation-advisor`) operator is enabled, the minio pod is not working correctly.

#### Solution

If you enabled the Mutation Advisor (`ibm-management-mutation-advisor`) operator when you installed {{site.data.keyword.cp4mcm_full_notm}}, you must complete the following steps to ensure the minio pod works correctly:

1. Run:
  ```
  oc edit csv ibm-management-mutation-advisor.v2.1.0 -n management-security-services
  ```
  {: codeblock}

2. Update the following section for `minio`. Change the value for `region` to the actual region where you deployed your cluster and change `zone` to either the zone where you deployed your cluster or the zone where you want Mutation Advisor to be scheduled. 
  ```
  volumeClaimTemplates:
  - metadata:
      creationTimestamp: null
      labels:
        billingType: hourly
        region: us-south
        zone: dal12
      name: datadir
  ```

### 2. Take action page is not displaying data.

  To solve this issue, complete the instructions in [Take Action pages show no data](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/mcm/troubleshoot/troubleshoot_takeactions.html).
