<!-- Name your file `getting-started.md` and include it in the Learn navigation group in your toc file. -->

---

copyright:
  years: 2019
lastupdated: "2019-09-29"

keywords: getting started tutorial, getting started, _cloud-pak-multicloud_manager, data

subcollection: writing

---
<!-- Copyright info and last updated date at top of file: REQUIRED
    The copyright and lastupdated info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be --- surrounded by 3 dashes ---
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    The value "lastupdated" must be followed by a machine date in quotes in the following format: "YYYY-MM-DD"
    The value for "years" must be indented 2 spaces under "copyright", followed by "lastupdated" which should start on its own non-indented line.
    Remember to delete this comment after you add your copyright info and last updated date, because the example dashes will cause CHKPII errors during translation packaging.

-->

<!-- Common attributes used in the template are defined as follows: -->
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

<!-- This template is for getting started with IBM Cloud Paks. It is a task template intended to document productive use of the offering.  -->




<!-- Please delete content examples and coding that you are not using for your service. -->

# Getting started with IBM Cloud Pak for Multicloud Management
{: #getting-started}

<!-- The title of your H1 should be Getting started with _cloud-pak_, where _cloud-pak_ is the non-trademarked short version conref. Include getting started and variations of your cloud pak name and function in the `meta keywords` values. See the example keywords above. -->

<!-- Short description: REQUIRED
The short description section should include one to two sentences describing why a user would want to use this Cloud Pak.
Briefly mention what the user's learning goal is and include the following SEO keywords in the title and/or the short description: IBM Cloud, CloudPakName. Be sure to use conversational style. For more details, see the guidance on conversational style in the Carbon Design System at http://www.carbondesignsystem.com/guidelines/content/general. Use the trademarked long version conref of your Cloud Pak name on first mention.

Example: -->

The IBM Cloud Pak for Multicloud Management, running on Red Hat OpenShift, provides consistent visibility, governance, and automation from on premises to the edge. Enterprises gain capabilities such as multicluster management, event management, application management and infrastructure management. Enterprises can use this IBM Cloud Pak to help increase operational efficiency that is driven by intelligent data, analysis, and predictive golden signals, and gain built-in support for their compliance management.
{:shortdesc}

<!-- Component section: REQUIRED
The component section includes a list of the offerings included in the Cloud Pak. Also include where a user can find more information on each offering.

Example: -->

## What's inside this Cloud Pak

IBM Cloud Pak for Multicloud Management includes the following products:

### IBM Multicloud Manager

IBM Multicloud Manager provides user visibility, application-centric management (governance, deployments, health, operations), and policy-based compliance across clouds and clusters. With {{site.data.keyword.mcm_notm}}, you have control of your Kubernetes clusters. You can ensure that your clusters are secure, operating efficiently, and delivering the service levels that applications expect. For more details about IBM Multicloud Manager, see the [documentation](www.ibm.com/support/knowledgecenter/SSFC4F_1.1.0/kc_welcome_mcm.html).

### IBM Cloud App Management

Monitor cloud and on-premises application environments with IBM Cloud App Management. Bridge your existing infrastructure into the cloud. For more details about IBM Cloud App Management, see the [documentation](https://www.ibm.com/support/knowledgecenter/SS8G7U_19.3.0/com.ibm.app.mgmt.doc/welcome.html?cp=SSFC4F_1.1.0).

### Cloud Event Management

You can visualize and manage multiple clusters when you install Event Management. By using Event Management, you can consolidate information from your monitoring systems and address problems. Events indicate that something happened on an application, service, or another monitored object. All events that are related to a single application, or to a particular cluster, are correlated with an incident. Event Management can receive events from various monitoring sources, either on-premises or in the cloud. Event Management is installed along with IBM Cloud App Management. For more details about Event Management, see the [documentation](https://www.ibm.com/support/knowledgecenter/SSURRN/com.ibm.cem.doc/index.html?cp=SSFC4F_1.1.0).

### IBM Cloud Automation Manager

IBM Cloud Automation Manager is a cloud management solution in IBM Cloud Private that automates provisioning of infrastructure and virtual machine applications across multiple cloud environments with optional workflow orchestration. For more details about IBM Cloud Automation Manager, see the [documentation](https://www.ibm.com/support/knowledgecenter/SS2L37_3.2.1.0/kc_welcome.html?cp=SSFC4F_1.1.0).

<!-- Task section: REQUIRED
The task section includes steps to get the Cloud Pak installed through IBM Cloud and next initial steps to get up and running.
- DO include the basic, most-common-use scenario steps to use the Cloud Pak.
- DO NOT repeat the UI from IBM Cloud catalog details page; instead, reference the pages or sections.
-->

<!-- Include a prerequisites paragraph for any prerequisites to be met. For example: REQUIRED -->
## Before you begin
- Before you can install the Cloud Pak, you must purchase a license through [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html). For part numbers, see [Passport Advantage part numbers](https://www.ibm.com/support/knowledgecenter/SSFC4F_1.1.0/cp/getting_started/part_numbers.html).

- You must have OpenShift Container Platform version 3.11 installed by using IBM Cloud Kubernetes Service so that the managed OpenShift service is supported.

* You must have a pre-configured StorageClass in OpenShift that can be used for installing the IBM Cloud Pak for Multicloud Management.

## Step 1. Configure your installation environment
{: #config-enviro}

Specify where you want to install the IBM Cloud Pak for Multicloud Management:

  1. Select the Red Hat OpenShift cluster where you want to deploy the IBM Cloud Pak for Multicloud Management.
  2. Enter or select the project where you want to deploy IBM Cloud Pak for Multicloud Management.

## Step 2. Configure your workspace
{: #config-workspace}

Specify how you will track and manage your installation from your IBM Cloud Schematics workspace:

  1. In the _Configure your workspace_ section, update the name and tag of the installation workspace.
  2. Specify any tags that you want to use for the installation. Specify multiple tags as a comma-separated list.

## Step 3. Complete the pre-installation check
{: #pre-install-check}

A Red Hat OpenShift cluster administrator must complete this step.

  - If you are not an administrator, use the Share link to share the script with your cluster administrator.
  - If you are a cluster administrator, click **Run script** to run the pre-installation check. Confirm that the script completes successfully.

## Step 4. Set the deployment values
{: #set-deploy-values}

Override the default deployment values:

  1. Configure the required deployment values for the OpenShift cluster that you have installed:

| Parameter | Description | Default |
| -------- | -------- | -------- |
| `defaultAdminUser`  |  Configure default admin user name.  |  |
| `defaultAdminPassword`  | Configure default admin user password. Password must be at least 32 characters by default, and can include only number, letter, and hyphens.|  |
| `isEnableMonitoringServices` | Configure if you enable monitoring services.  |  `enabled` |
|  `isEnableMeteringServices`  |  Configure if you enable metering services.  |  `disabled`  |
| `storageClass`  |  Configure storage class. | `ibmc-block-bronze` |
{: caption="Table 1. Deployment values" caption-side="top"}

## Step 5. Install IBM Cloud Pak for Multicloud Management
{: #install-cp4mcm}

1. Ensure that you have assigned a license for the IBM Cloud Pak for Multicloud Management to the deployment.
2. Confirm that you have read and agree to the license agreements.
3. Click **Install**.
<!-- For code examples:
- use three backticks ahead of and after the example (```)
- For copyable code snippet, multi-line, include {: codeblock} following the last set of backticks. A copy button will display in framework in output.
- For copyable command, single line, include {: pre} following the last set of backticks. When displayed, it will show "$" at the beginning of the command example and a copy button, but the copy button will include just the command example.
- For non-copyable output snippet, include {: screen} following the last set of backticks.
 -->

## Next steps

When the installation completes, you can access your IBM Cloud Pak for Multicloud Management deployment with the provided URL.

  1. Log in the IBM Cloud Pak for Multicloud Management management console by using the admin user name and password that you configured during the installation.
  2. Optional: Install the optional components in the IBM Cloud Pak for Multicloud Management.
    - [Installing IBM Cloud App Management](https://www.ibm.com/support/knowledgecenter/SS8G7U_19.3.0/com.ibm.app.mgmt.doc/content/install_mcm.html?cp=SSFC4F_1.1.0)
    - [Installing IBM Cloud Automation Manager](https://www.ibm.com/support/knowledgecenter/SS2L37_3.2.1.0/cam_install_EE_main.html?cp=SSFC4F_1.1.0)

<!-- Add the topic to your `toc` file:


{:navgroup: .navgroup}
{:topicgroup: .topicgroup}

{: .toc subcollection="<Folder_name>" audience="oss" category="<category>" href="/docs/<folder_name>?topic=<subcollection>getting-started"}
<Cloud Pak Name>

    {: .navgroup id="learn"}
    getting-started.md

    {: .topicgroup}
    Related links
        [Link text](link URL)
    {: .navgroup-end}

    {: .navgroup id="reference"}
    Reference
    [Link text](link URL)
    {: .navgroup-end}
-->
