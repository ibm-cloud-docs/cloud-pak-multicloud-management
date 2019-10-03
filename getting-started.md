---

copyright:
  years: 2019
lastupdated: "2019-09-29"

keywords: getting started tutorial, getting started, cloud-pak-multicloud_management,

subcollection: writing

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

The {{site.data.keyword.cp4mcm_full_notm}}, running on Red Hat OpenShift, provides consistent visibility, governance, and automation from on-premises to the edge. Enterprises gain capabilities such as multicluster management, event management, application management and infrastructure management. Enterprises can use this Cloud Pak to help increase operational efficiency that is driven by intelligent data, analysis, and predictive golden signals, and gain built-in support for their compliance management. For more details, see the [documentation](https://www.ibm.com/support/knowledgecenter/SSFC4F/product_welcome_cloud_pak.html).
{:shortdesc}

## What's inside this Cloud Pak

The {{site.data.keyword.cp4mcm_full_notm}} includes the following components:

### {{site.data.keyword.IBM_notm}} Multicloud Manager

{{site.data.keyword.IBM_notm}} Multicloud Manager provides user visibility, application-centric management (governance, deployments, health, operations), and policy-based compliance across clouds and clusters. With {{site.data.keyword.IBM_notm}} Multicloud Manager, you have control of your Kubernetes clusters. You can ensure that your clusters are secure, operating efficiently, and delivering the service levels that applications expect. For more details about IBM Multicloud Manager, see the [documentation](www.ibm.com/support/knowledgecenter/SSFC4F_1.1.0/kc_welcome_mcm.html).

### {{site.data.keyword.IBM_notm}} Cloud App Management

Monitor cloud and on-premises application environments with {{site.data.keyword.IBM_notm}} Cloud App Management. Bridge your existing infrastructure into the cloud. For more details about {{site.data.keyword.IBM_notm}} Cloud App Management, see the [documentation](https://www.ibm.com/support/knowledgecenter/SS8G7U_19.3.0/com.ibm.app.mgmt.doc/welcome.html?cp=SSFC4F_1.1.0).

### Cloud Event Management

You can visualize and manage multiple clusters when you install Event Management. By using Event Management, you can consolidate information from your monitoring systems and address problems. Events indicate that something happened on an application, service, or another monitored object. All events that are related to a single application, or to a particular cluster, are correlated with an incident. Event Management can receive events from various monitoring sources, either on-premises or in the cloud. Event Management is installed along with {{site.data.keyword.IBM_notm}} Cloud App Management. For more details about Event Management, see the [documentation](https://www.ibm.com/support/knowledgecenter/SSURRN/com.ibm.cem.doc/index.html?cp=SSFC4F_1.1.0).

### {{site.data.keyword.IBM_notm}} Cloud Automation Manager

{{site.data.keyword.IBM_notm}} Cloud Automation Manager is a cloud management solution that automates provisioning of infrastructure and virtual machine applications across multiple cloud environments with optional workflow orchestration. For more details about {{site.data.keyword.IBM_notm}} Cloud Automation Manager, see the [documentation](https://www.ibm.com/support/knowledgecenter/SS2L37_3.2.1.0/kc_welcome.html?cp=SSFC4F_1.1.0).

## Before you begin
- Before you can install the Cloud Pak, you must purchase a license through [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html). For part numbers, see [Passport Advantage part numbers](https://www.ibm.com/support/knowledgecenter/SSFC4F_1.1.0/cp/getting_started/part_numbers.html).

- You must have {{site.data.keyword.openshiftshort}} Container Platform version 3.11 installed by using {{site.data.keyword.IBM_notm}} Cloud Kubernetes Service so that the managed {{site.data.keyword.openshiftshort}} service is supported.

* You must have a pre-configured StorageClass in {{site.data.keyword.openshiftshort}} that can be used for installing the {{site.data.keyword.cp4mcm_full_notm}}.

### Minimum hardware requirements

| Nodes | Memory  | CPU  |
|----|---|----|
| 1 | 32 GB | 16 cores |

**Notes:** If you are going to install {{site.data.keyword.IBM_notm}} Cloud App Management and {{site.data.keyword.IBM_notm}} Cloud Automation Manager with the {{site.data.keyword.cp4mcm_full_notm}}, you need another two nodes.

### Minimum storage requirements

Mandatory: 2 PV

| Component | PV |
|-----|-----|
| MongoDB | 1 |
| {{site.data.keyword.IBM_notm}} Multicloud Manager etcd | 1 |

Optional: 4 PV

| Component | PV  |
|------|------|
| Logging   |  1  |
| VA  |    3  |

## Step 1. Configure your installation environment
{: #config-enviro}

Specify where you want to install the {{site.data.keyword.cp4mcm_full_notm}}:

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

## Step 4. Set the deployment values
{: #set-deploy-values}

Override the default values by configuring the required deployment values for the {{site.data.keyword.openshiftshort}} cluster that you have installed:

| Parameter | Description | Default |
| -------- | -------- | -------- |
| `defaultAdminUser`  |  Configure default admin user name.  |  |
| `defaultAdminPassword`  | Configure default admin user password. Password must be at least 32 characters by default, and can include only number, letter, and hyphens.|  |
| `isEnableMonitoringServices` | Configure if you enable monitoring services.  |  `enabled` |
|  `isEnableMeteringServices`  |  Configure if you enable metering services.  |  `disabled`  |
| `storageClass`  |  Configure storage class. | `ibmc-block-bronze` |
{: caption="Table 1. Deployment values" caption-side="top"}

## Step 5. Install the {{site.data.keyword.cp4mcm_full_notm}}
{: #install-cp4mcm}

1. Ensure that you have assigned a license for the {{site.data.keyword.cp4mcm_full_notm}} to the deployment.
2. Confirm that you have read and agree to the license agreements.
3. Click **Install**.

## Next steps

When the installation completes, you can access your {{site.data.keyword.cp4mcm_full_notm}} deployment with the provided URL.

  1. Log in the {{site.data.keyword.cp4mcm_full_notm}} management console by using the admin user name and password that you configured during the installation.
  2. Optional: Install the optional components in the {{site.data.keyword.cp4mcm_full_notm}}.
       - [Installing IBM Cloud App Management](https://www.ibm.com/support/knowledgecenter/SS8G7U_19.3.0/com.ibm.app.mgmt.doc/content/install_mcm.html?cp=SSFC4F_1.1.0)
       - [Installing IBM Cloud Automation Manager](https://www.ibm.com/support/knowledgecenter/SS2L37_3.2.1.0/cam_install_EE_main.html?cp=SSFC4F_1.1.0)
