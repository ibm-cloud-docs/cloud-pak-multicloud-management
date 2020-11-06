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

# Option 1. Deploying {{site.data.keyword.inf_man_notm}} as a containerized deployment (podified).

Complete these steps to install {{site.data.keyword.inf_man_notm}} as a containerized deployment.
{: shortdesc}

- [Prerequisites](#pre-requisites)
- [Step 1. Deploy the ibm-management-im-install operand](#deploy-install-operand)
- [Step 2. Create a secret for functional operators usage](#oper_secret)
- [Step 3. Run the automation script to enable integration](#auto_script)

## Prerequisites
{: #pre-requisites}
- Ensure you enable the operators for `Infrastructure management` by opening the {{site.data.keyword.mcm}} installation YAML file. Locate the `pakModules` section, and change `enabled: false` to `enabled: true`. Enable these {{site.data.keyword.inf_man_notm}}-related operators:

  - `ibm-management-im-install` for {{site.data.keyword.inf_man_notm}}. For more information, see [Infrastructure management](../infrastructure/infra_mgmt_intro.md).

  - `ibm-management-infra-vm` for provisioning virtual machines and instances. For more information, see [Provisioning Virtual Machines and Instances](../../Infra_mgmt/provisioning_virtual_machines_and_hosts/index.md).

  - `ibm-management-infra-grc` for {{site.data.keyword.inf_man_notm}} policies and profiles. For more information, see [Policies and Profiles Guide](../../Infra_mgmt/policies_and_profiles_guide/index.md).

  For more information, see [Enabling operators after IBM Cloud Pak for Multicloud Management installation](../../install/enable_operator.md).

  **Important:** Before you deploy these operands, you must complete the OIDC (OpenID Connect) configuration for {{site.data.keyword.inf_man_notm}}.

- [Configuring OIDC for {{site.data.keyword.inf_man_notm}}](oidc_config_infra_mgmt.md).
  
  **Note:** Only OIDC-based authentication (OpenID Connect) is supported. The configuration of OpenID Connect (OIDC) is required for integration with {{site.data.keyword.inf_man_notm}} and {{site.data.keyword.cloud_pak_mcm_notm}}.
    
## Step 1. Deploy the ibm-management-im-install operand
{: #deploy-install-operand}

After the OIDC configuration is completed for {{site.data.keyword.inf_man_notm}}, you can apply the CR to install an instance of {{site.data.keyword.inf_man_notm}} by using the {{site.data.keyword.inf_man_notm}} installation operator. You can install an instance of {{site.data.keyword.inf_man_notm}} by using the {{site.data.keyword.ocp}} console or by using the CLI (command-line tools).

* Specify the Infrastructure management route `<YOUR_IM_HTTPD_ROUTE>` in the CR field `applicationDomain`.
    *  applicationDomain: *inframgmtinstall.apps.mycluster.os.fyre.ibm.com*
    
    **Note:** Use the same value for `<YOUR_IM_HTTPD_ROUTE>` that you used in the OIDC configuration in the CR and operand deployments. `<YOUR_IM_HTTPD_ROUTE>` is the main URL of the {{site.data.keyword.inf_man_notm}} installation.
* If deploying as part of a multi-region setup, specify a unique number in the CR field `databaseRegion`.  If not specified, this value will default to 0.
    *  databaseRegion: *99*
* Add the following fields to enable OIDC. Reference the secret name created in the CR field `httpdAuthConfig`
    *  httpdAuthenticationType: openid-connect
    *  httpdAuthConfig: imconnectionsecret
    *  enableSSO: true
* Reference the default admin group in the CR field `initialAdminGroup`. Upon application of the CR, this group is created in {{site.data.keyword.inf_man_notm}}. Specify an existing group in your LDAP configuration.
    *  initialAdminGroupName: `<YOUR_LDAP_GROUP>`
    
    **Important:** This LDAP group is created in {{site.data.keyword.inf_man_notm}} to match your existing LDAP group by name, and assigns the group account roles. At least one group to which the users belong in LDAP that {{site.data.keyword.cloud_pak_mcm_notm}} is configured to use must be specified for `<YOUR_LDAP_GROUP>`. For example, you have an existing LDAP group that is named group100 and a user with the username user100 is a member of the group. You enter group100 for the value of `<YOUR_LDAP_GROUP>`.
* Add the image pull secret `imagePullSecret` to access the {{site.data.keyword.inf_man_notm}} images required by the installer.
    *  imagePullSecret: *my-pull-secret*
     
       **Note:** `imagePullSecret` is the same secret that was created for entitled registry. For more information, see [Create the entitled registry secret](../../install/prep_online.md#er)

Example CR:

```yaml
apiVersion: infra.management.ibm.com/v1alpha1
kind: IMInstall
metadata:
  labels:
    app.kubernetes.io/instance: ibm-infra-management-install-operator
    app.kubernetes.io/managed-by: ibm-infra-management-install-operator
    app.kubernetes.io/name: ibm-infra-management-install-operator
  name: im-iminstall
  namespace: management-infrastructure-management
spec:
  applicationDomain: inframgmtinstall.apps.mycluster.os.fyre.ibm.com
  databaseRegion: 99
  imagePullSecret: my-pull-secret
  httpdAuthenticationType: openid-connect
  httpdAuthConfig: imconnectionsecret
  enableSSO: true
  initialAdminGroupName: inframgmtusers
  license:
    accept: true
  orchestratorInitialDelay: '2400'
```
Apply the CR by using the {{site.data.keyword.open_s}} console or the CLI.

### Install by using the OpenShift Console Install

1. Verify that the {{site.data.keyword.inf_man_notm}} operator is already installed.
2. Create an instance to deploy {{site.data.keyword.inf_man_notm}}.
   1. Browse to **Operators > Installed Operators**.
   2. Click **IBM Infrastructure Management Install**.
   3. Click **Create Instance**.
   4. Select **Update YAML** (see the example CR)
   5. Click **Create**
3. Verify that pods are running (it can take up to 15 minutes for pods to start).

### Install by using the CLI

1. Verify that the {{site.data.keyword.inf_man_notm}} operator is already installed.
2. Create a `infra.management.ibm.com_v1alpha1_iminstall_cr.yaml` file (see the example CR).
3. Update the YAML file.
4. Create an instance to deploy {{site.data.keyword.inf_man_notm}} by running the command:
    ~~~
    $ oc apply -f infra.management.ibm.com_v1alpha1_iminstall_cr.yaml
    ~~~
    {: codeblock}
5. Verify that pods are running (it can take up to 15 minutes for pods to start).

## Step 2. Create a secret for functional operators usage
{: #oper_secret}

To use the functional operators (`ibm-management-infra-vm`, and `ibm-management-infra-grc`) with {{site.data.keyword.inf_man_notm}}, you need a Connection CR. You can use the example `imconnection.yaml` file to create a default connection and update as needed.
**Note:** The default connection name of `imconnection` is used by the operators unless otherwise specified in the CR.

1. Create a `imconnection.yaml` file based on the example to create the connection CR in the namespace `management-infrastructure-management`.

    ```yaml
    apiVersion: infra.management.ibm.com/v1alpha1
    kind: Connection
    metadata:
      annotations:
        BypassAuth: "true"
      labels:
       controller-tools.k8s.io: "1.0"
      name: imconnection
      namespace: "management-infrastructure-management"
    spec:
      cfHost: web-service.management-infrastructure-management.svc.cluster.local:3000
    ```
2. Run the oc command to create the connection.
    ```
    oc create -f imconnection.yaml
    ```
    {: codeblock}

## Step 3. Integrating {{site.data.keyword.inf_man_notm}} with {{site.data.keyword.cloud_pak}} {{site.data.keyword.gui}}
{: #auto_script}

Enable navigation to {{site.data.keyword.inf_man_notm}} with {{site.data.keyword.cloud_pak}} {{site.data.keyword.gui}}.

Complete the following steps to enable navigation to {{site.data.keyword.inf_man_notm}}:

Download the part number: CC7X4EN
 
 | Description                                                                      | File name                               | Passport Advantage part number |
   |----------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
   | Automation navigation for IBM Cloud Pak® for Multicloud Management 2.1 | automation-navigation-updates.sh | CC7X4EN |

1. Obtain the menu customization script, `automation-navigation-updates.sh`, from [IBM Passport Advantage® ![Opens in a new tab](../../images/icons/launch-glyph.svg "Opens in a new tab")](https://www.ibm.com/software/passportadvantage/){: new_window} website. You must run the script on a {{site.data.keyword.linux_notm}} operating system. 

2. Install and authenticate `kubectl`. For more information, see [Managing your clusters with {{site.data.keyword.mcm_notm}} ![Opens in a new tab](../../images/icons/launch-glyph.svg "Opens in a new tab")](../../kubectl/install_kubectl.md).

3. Download and configure the JQ tool by using the following commands:
   ```
   wget -O jq https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64

   chmod +x ./jq

   mv jq /usr/bin
   ```
   For more information, see [Download jq ![Opens in a new tab](../../images/icons/launch-glyph.svg "Opens in a new tab")](https://stedolan.github.io/jq/download/){: new_window}.

4. Run the following commands to enable navigation to your {{site.data.keyword.inf_man_notm}} instance:

   ```
   chmod 755 ./automation-navigation-updates.sh

   ./automation-navigation-updates.sh -p management-infrastructure-management
   ```
   {: codeblock}

   * The `automation-navigation-updates.sh` expects the default namespace `management-infrastructure-management`. If you use a different namespace to install {{site.data.keyword.inf_man_notm}}, you must pass the namespace as a parameter when you run the command. Example:
   ```
   ./automation-navigation-updates.sh -p <namespace>
   ```
   {: codeblock}

5. Verify that the {{site.data.keyword.inf_man_notm}} instance is in the {{site.data.keyword.cloud_pak}} {{site.data.keyword.gui}} navigation menu. From the {{site.data.keyword.cloud_pak}} menu, click **Automate infrastructure** > **Infrastructure management**.