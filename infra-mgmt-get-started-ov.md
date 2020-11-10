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

# Getting started with Infrastructure management
Infrastructure management was previously named IBM Red Hat CloudForms. Infrastructure management delivers the insight, control, and automation enterprises need to address the challenges of managing virtual environments. This technology enables enterprises to improve visibility and control with virtual infrastructures.

## Overview
The instructions for deploying Infrastructure management with IBM Cloud Pak for Multicloud Management are identical for versions 2.0.0 and 2.1.0. If you are installing 2.0.0, download and use the 2.0 part numbers. If you are installing 2.1.0, download and use the 2.1 part numbers. You must use the same version for Infrastructure management that match with your IBM Cloud Pak for Multicloud Management version.

## Passport Advantage part numbers
Use these tables to find the files that you need from IBM Passport Advantage and the corresponding part numbers.
{: shortdesc}

## Downloading packages
{: #download}

Complete the following procedure to download your packages:

1. Browse to [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/pao_customer.html) and log in with your IBM ID.
2. Find your part number.
3. Search for your files by using the part numbers that are listed in the tables.
4. Download the files to a directory on your computer.

## Infrastructure management packages
| Description                                                                      | File name                               | Passport Advantage part number |
|----------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
|IBM Cloud Pak for Multicloud Management 2.1 Infrastructure management for Red Hat OpenStack Platform |cp4mcm-im-rhos-2.1.x86_64.qcow2|CC7X2EN|
|Automation navigation for IBM Cloud Pak for Multicloud Management 2.1|automation-navigation-updates.sh|CC7X4EN|
|IBM Cloud Pak for Multicloud Management 2.0 Infrastructure management for Red Hat OpenStack Platform |cp4mcm-im-rhos-2.0.x86_64.qcow2|CC732EN|
|Automation navigation for IBM Cloud Pak for Multicloud Management 2.0|automation-navigation-updates.sh|CC734EN|
{: caption="Table 1. Infrastructure managment packages" caption-side="top"}

## Installing Infrastructure management - the options

Two options are available to install and deploy operands for Infrastructure management.

### Option 1. Install Infrastructure management as a containerized deployment (podified).
   - Installing Infrastructure management (podified) in the hub cluster consists of these steps.
      1. Configure OIDC integration between IBM Cloud Pak for Multicloud Management and Infrastructure management.
      2. Deploy the ibm-management-im-install operand.
      3. Create a connection for the functional operators.
      4. Download and run the automation script to enable navigation to Infrastructure management with IBM Cloud Pak for Multicloud Management.
   - To install Infrastructure management as a containerized deployment, see [Getting started deploying Infrastructure management as a containerized deployment (podified)](https://test.cloud.ibm.com/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-getting-started-deploying-infrastructure-management-as-a-containerized-deployment-podified-).

### Option 2. Install Infrastructure management as a virtual machine appliance.
   - Installing Infrastructure management as a virtual machine appliance consists of these steps.
      1. Download the Infrastructure management appliance package for your environment.
      2. Install and configure the Infrastructure management appliance.
      3. Configure OIDC integration between IBM Cloud Pak for Multicloud Management and the Infrastructure management appliance.      
   - To install Infrastructure management as a virtual machine appliance, see [Getting started deploying Infrastructure management as a virtual machine appliance](https://test.cloud.ibm.com/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-getting-started-deploying-infrastructure-management-as-a-virtual-machine-appliance). 

The Infrastructure management-related operators.

  - `ibm-management-im-install` for Infrastructure management. For more information, see [Infrastructure management](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/mcm/infrastructure/infra_mgmt_intro.html).

  - `ibm-management-infra-vm` for provisioning virtual machines and instances. For more information, see [Provisioning Virtual Machines and Instances](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/Infra_mgmt/provisioning_virtual_machines_and_hosts/index.html).

  - `ibm-management-infra-grc` for Infrastructure management. For more information, see [Governance, risk, and compliance](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/mcm/compliance/compliance_intro.html).

