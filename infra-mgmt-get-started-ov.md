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

Two options are available to install and deploy operands for Infrastructure management.

### Option 1. Install Infrastructure management as a containerized deployment (podified).
   - Installing Infrastructure management (podified) in the hub cluster consists of these steps.
      1. Configure OIDC integration between IBM Cloud Pak for Multicloud Management and Infrastructure management.
      2. Deploy the ibm-management-im-install operand.
      3. Create a connection for the functional operators.
      4. Download and run the automation script to enable navigation to Infrastructure management with {{site.data.keyword.cloud_pak}}.
   - To install Infrastructure management as a containerized deployment, see [Deploying Infrastructure management as a containerized deployment (podified)](infra-mgmt-get-started-pod-v2.md)

### Option 2. Install Infrastructure management as a virtual machine appliance.
   - Installing Infrastructure management as a virtual machine appliance consists of these steps.
      1. Download the Infrastructure management appliance package for your environment.
      2. Install and configure the Infrastructure management appliance.
      3. Configure OIDC integration between IBM Cloud Pak for Multicloud Management and the Infrastructure management appliance.      
   - To install Infrastructure management as a virtual machine appliance, see [Deploying Infrastructure management as a virtual machine appliance](https://cloud.ibm.com/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-option-2-deploying-infrastructure-management-as-a-virtual-machine-appliance). 

The Infrastructure management-related operators.

  - `ibm-management-im-install` for Infrastructure management. For more information, see [Infrastructure management](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/mcm/infrastructure/infra_mgmt_intro.html).

  - `ibm-management-infra-vm` for provisioning virtual machines and instances. For more information, see [Provisioning Virtual Machines and Instances](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/Infra_mgmt/provisioning_virtual_machines_and_hosts/index.html).

  - `ibm-management-infra-grc` for Infrastructure management. For more information, see [Governance, risk, and compliance](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/mcm/compliance/compliance_intro.html).

