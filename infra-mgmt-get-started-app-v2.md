---

copyright:
  years: 2020
lastupdated: "2020-10-27"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:child: .link .ulchildlink}
{:childlinks: .ullinks}


# Option 2. Deploying {{site.data.keyword.inf_man_notm}} as a virtual machine appliance

Complete these steps to install {{site.data.keyword.inf_man_notm}} as a virtual machine appliance.
{: shortdesc}
   
- [Prerequisites](#pre-requisites)
- [Step 1. Download the appliance image](#download-appliance)
- [Step 2. Install and configure the appliance](#app-install)
- [Step 3. Configure OIDC for {{site.data.keyword.inf_man_notm}} appliance](#config-oidc-app)

## Prerequisites
{: #pre-requisites}
- 
- Ensure you enable the operators for `Infrastructure management` by opening the {{site.data.keyword.mcm}} installation YAML file. Locate the `pakModules` section, and change `enabled: false` to `enabled: true`. Enable these {{site.data.keyword.inf_man_notm}}-related operators:

  - `ibm-management-im-install` for {{site.data.keyword.inf_man_notm}}. For more information, see [Infrastructure management](infra_mgmt_intro.md).

  - `ibm-management-infra-vm` for provisioning virtual machines and instances. For more information, see [Provisioning Virtual Machines and Instances](../../Infra_mgmt/provisioning_virtual_machines_and_hosts/index.md).

  - `ibm-management-infra-grc` for {{site.data.keyword.inf_man_notm}} policies and profiles. For more information, see [Policies and Profiles Guide](../../Infra_mgmt/policies_and_profiles_guide/index.md).

    For more information, see [Enabling operators after IBM Cloud Pak for Multicloud Management installation](../../install/enable_operator.md).

- You must configure and connect an LDAP directory with {{site.data.keyword.cloud_pak_mcm_notm}}. You must have an LDAP group in your configuration for {{site.data.keyword.cloud_pak_mcm_notm}} with users defined who will access {{site.data.keyword.inf_man_notm}}.      

## Step 1. Download the {{site.data.keyword.inf_man_notm}} appliance package for your environment.
{: #download-appliance}
   1. Go to the {{site.data.keyword.ippa}} Online tab at [{{site.data.keyword.ppa_notm}} ![Opens in a new tab](../../images/icons/launch-glyph.svg "Opens in a new tab")](https://www.ibm.com/software/passportadvantage/pao_customer.html){: new_window} and log in with your {{site.data.keyword.IBM_notm}} ID.
   2. Find your part number.
   3. Search for the files by using the part numbers that are listed in the table.
   4. Download the files to a directory on your system.

### {{site.data.keyword.inf_man_notm}} appliance packages
{: #inf_man_files}

| Description                                                                      | File name                               | Passport Advantage part number |
|----------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
|IBM Cloud Pak for Multicloud Management 2.1 Infrastructure management for Red Hat OpenStack Platform |cp4mcm-im-rhos-2.1.x86_64.qcow2|CC7X2EN|

{: caption="Table 1. {{site.data.keyword.inf_man_notm}} Appliance packages" caption-side="top"}

## Step 2. Install and configure the {{site.data.keyword.inf_man_notm}} appliance. 
{: #app-install}
Follow the steps in [Install the {{site.data.keyword.inf_man_notm}} appliance](infra_mgmt_install_upgrade.md) for your virtual environment.

## Step 3. Configure OIDC integration between {{site.data.keyword.cloud_pak_mcm_notm}} and the {{site.data.keyword.inf_man_notm}} appliance.
{: #config-oidc-app}
Follow the steps in [Configuring OIDC for {{site.data.keyword.inf_man_notm}} appliance only](oidc_config_im_appliance.md).

   **Note:**
   
   Only OIDC-based authentication (OpenID Connect) is supported. The configuration of OpenID Connect (OIDC) is required for integration with {{site.data.keyword.inf_man_notm}} and {{site.data.keyword.cloud_pak_mcm_notm}}.




---

copyright:
  years: 2020
lastupdated: "2020-11-05"

keywords: getting started tutorial, getting started, infrastructure management

subcollection: cloud-pak-multicloud-management

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Getting started with Infrastructure management in IBM Cloud
{: #cf-getting-started}

CloudForms delivers the insight, control, and automation enterprises need to address the challenges of managing virtual environments. CloudForms enables enterprises with existing virtual infrastructures to improve visibility and control, and those just starting virtualization deployments to build and operate a well-managed virtual infrastructure. For more information, see the [CloudForms product documentation](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/5.0/).
You can install CloudForms as a virtual appliance in IBM Cloud. 
{:shortdesc}

## Before you begin

- Before you can install CloudForms, you must download the images from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/index.html). Download these two part numbers:
  
| Description                                                                      | File name                               | Passport Advantage part number |
|----------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
| Red Hat CloudForms 5 for Red Hat OpenStack Platform | cfme-rhos-5.11.4.x86_64.qcow2 |   CC5W9EN  |
| Automation navigation for IBM Cloud Pak® for Multicloud Management 1.3 | automation-navigation-updates.sh | CC66KEN  |

- For the list of all part numbers, see [Passport Advantage part numbers](https://www.ibm.com/support/knowledgecenter/en/SSFC4F_1.3.0/about/part_numbers.html).
  
- You must have an IBM Cloud user account with the following roles: 
![Figure showing the required roles for an IBM Cloud user account.](images/required_roles.png){: caption="Figure 1. Required roles for IBM Cloud user account" caption-side="bottom"}

- You must have {{site.data.keyword.cp4mcm_full_notm}} installed. For more information, see [Getting started with {{site.data.keyword.cp4mcm_full_notm}}](https://test.cloud.ibm.com/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-getting-started)  


## Step A. Setting up the Custom image for CloudForms in IBM Cloud
{: #config-image}

Create a custom Linux-based image to deploy CloudForms as a virtual server instance in IBM Cloud.

1. If you don't already have an instance of IBM Cloud Object Storage, see [Getting started with IBM Cloud Object Storage](https://cloud.ibm.com/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)

    ![Figure showing example Cloud Object Storage created.](images/cloud_object_storage.png){: caption="Figure 2. Example Cloud Object Storage created" caption-side="bottom"}

    You must also create a bucket in IBM Cloud Object Storage to store your images.
    ![Figure showing example standard type bucket created.](images/buckets.png){: caption="Figure 3. Example Standard type bucket created" caption-side="bottom"}


2. Upload the CloudForms installation image (file name: `cfme-rhos-5.11.4.x86_64.qcow2`) to your IBM Cloud Object Storage. Select your bucket and click Add Objects to upload the images. For more information, see [Uploading data by using the console](https://cloud.ibm.com/docs/services/cloud-object-storage?topic=cloud-object-storage-upload#upload-console). **Note:** You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB.  
![Figure showing example that uses Aspera uploaded file to bucket.](images/upload_images_to_bucket.png){: caption="Figure 4. Example using Aspera uploaded file to bucket" caption-side="bottom"}

3. From IBM Cloud Identity and Access Management (IAM), create an authorization between the Virtual Private Cloud (VPC) Infrastructure (source service) > Image Service for VPC (resource type) and Cloud Object Storage (target service). For more information, see [Create an authorization](https://cloud.ibm.com/docs/iam?topic=iam-serviceauth#serviceauth).
    
    **Important**: The configuration must be set up as this example or permissions can fail. 
    ![Figure showing example IAM authorization.](images/service_auth_vpc.png){: caption="Figure 5. Example IAM authorization" caption-side="bottom"}

4. Create a generation 2 Virtual Private Cloud (**Must be generation 2**). For more information, see [Create a VPC](https://cloud.ibm.com/docs/vpc?topic=vpc-getting-started#create-and-configure-vpc)
  
    a. Create a VPC - The VPC must be in the same resource group and region as your bucket.

    b. Create subnets in one or more zones. You can create subnets in suggested prefix ranges or in your own IP ranges that you bring to IBM Cloud.

    c. Attach a public gateway if you want to allow all resources in a subnet to communicate with the public internet.

    ![Figure showing example VPC.](images/service_auth_vpc.png){: caption="Figure 6. Example VPC" caption-side="bottom"}

5. Configure an access control list (ACL) to limit the subnet's inbound and outbound traffic.

    ![Figure showing example ACL.](images/config_ACL.png){: caption="Figure 7. Example ACL" caption-side="bottom"}

6. Import the CloudForms installation images from the bucket into the VPC.
  
    a. Browse to **VPC Infrastructure** > **Compute** > **Custom images** and select **import custom image**.

    b. Enter a name.

    c. Select a resource group.

    d. Select the region.

    ![Figure showing importing of custom image.](images/import_custom_image.png){: caption="Figure 8. Example import of custom image" caption-side="bottom"}

    e. Select your Cloud Object Storage and bucket based on your authorization that is created in step 3.

    f. Select your qcow2 image (custom image).

    g. Select the Red Hat Enterprise for Operating system.

    h. Click **Import custom image**.

    ![Figure showing example of custom image imported.](images/select_qcow2_image.png){: caption="Figure 9. Example of custom image imported" caption-side="bottom"}

    ![Figure showing example of custom image listing after successful image creation.](images/results_vpc_images.png){: caption="Figure 10. Example of custom image listing after successful image creation" caption-side="bottom"}

7. Create a virtual server from the custom image by clicking the "three dot menu" of that image, then selecting "New virtual server".
![Figure showing select three dot menu for New virtual server.](images/select_new_server.png){: caption="Figure 11. Select New virtual server from the three dot menu" caption-side="bottom"}
  
   a. Enter your name. Select the Virtual private cloud and Resource group.
   ![Figure showing New virtual server for VPC.](images/new_virtual_server.png){: caption="Figure 12. Enter your name, select the Virtual private cloud and Resource group" caption-side="bottom"}

   b. Select your region.

   c. Select the custom image that you imported.

   d. Use Memory Profile (2 vcpus, 16 gb ram, 4 gps).

   e. Add an ssh key. You can use a public key. For more information, see: [Locating or generating your SSH key](https://cloud.ibm.com/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-ssh-keys#locating-or-generating-your-ssh-key).

   ![Figure showing add ssh key.](images/ssh_keys.png){: caption="Figure 13. Add an ssh key" caption-side="bottom"}

   f. Add storage to your virtual service. For example, 100 gigabytes. This volume is needed to configure the CloudForms appliance. 

   **Note:** Make sure that the data volume name is unique and not named the same as another volume across your virtual server instances.

   ![Figure showing example Data volumes.](images/data_volumes.png){: caption="Figure 14. Example Data volumes" caption-side="bottom"}

    g. Select create virtual server instance. 
 
8. Update the security group that allows inbound and outbound traffic. Open the server instance, go down to the Network interfaces section, and then modify the security group.

   ![Figure showing example security group that allows inbound and outbound traffic.](images/security_inbound_outbound.png){: caption="Figure 15. Example security group that allows inbound and outbound traffic" caption-side="bottom"}

9. Assign the floating IP address:

   ![Figure showing example Floating IP address.](images/floating_ip_vpc.png){: caption="Figure 16. Example Floating IP address assigned" caption-side="bottom"}


## Step B. Setting up the CloudForms appliance
{: #config-cloudforms-appliance}

1. Use the `ssh` command to connect to your virtual server instance (appliance) by using the floating IP address. Log in with a username of `root` and the default password `smartvm`. The Bash prompt for the root user is displayed.
  
   Example ssh as root user:
   ```
   ssh root@<host_ip_address>
   ```
2. Enter the `appliance_console` command. The CloudForms appliance summary screen is displayed.

   ![Figure showing example appliance_console.](images/setup_appliance.png){: caption="Figure 17. Welcome to the Appliance Console" caption-side="bottom"}

3. Press Enter to manually configure settings.

    **Note:** Networking is already configured. You can skip this step.

4. Select _5) Configure database_ from the menu.

    - You are prompted to create or fetch an encryption key.
    If this instance is the first CloudForms appliance, select _1) Create key_.
    
    - If this is not the first CloudForms appliance, select _2) Fetch key_ from remote system to fetch the key from the first appliance. For worker and multi-region setups, use this option to copy key from another appliance.

    **Note:** All CloudForms appliances in a multi-region deployment must use the same key.

5. Select _1) Create Internal Database_ for the database location.

6. Choose a disk for the database. The disk can be either a disk you attached previously, or a partition on the current disk.

    **Important:** Best practice is using a separate disk for the database.
    
    If an unpartitioned disk is attached to the virtual machine, the dialog shows the options. For example,
    ```
    1) /dev/vdb: 20480
    2) Don't partition the disk
    ```
    - Enter 1 to choose /dev/vdb for the database location. This option creates a logical volume by using this device and mounts the volume to the appliance in a location appropriate for storing the database. The default location is /var/lib/pgsql, which can be found in the environment variable $APPLIANCE_PG_MOUNT_POINT.
    
    - Enter 2 to continue without partitioning the disk. A second prompt confirms this choice. Selecting this option results in using the root file system for the data directory (not advised in most cases).

7. Enter N for "Should this appliance run as a stand-alone database server?"
    - Select N to configure the appliance with the full administrative user interface.

8. When prompted, enter a unique number (01-99) to create a new region.

    **Important:** Creating a new region deletes any existing data on the chosen database.

9.  Create and confirm a password for the database.

    CloudForms configures the internal database. This takes a few minutes. 

10. Once CloudForms is installed, you can log in and complete administrative tasks.
    - Log in to Red Hat CloudForms for the first time by:
    - Select the URL for the login screen. For example,  `https://xx.xx.xx.xx` on the virtual server instance, where `xx.xx.xx.xx` is the floating IP.
    - Enter the default credentials (Username: admin | Password: smartvm) for the initial login.
    - Click Login.
  
    For more information, see: [Configuring CloudForms](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/5.0/html/installing_red_hat_cloudforms_on_red_hat_openstack_platform/configuring-cloudforms)


## Step C. Integrating CloudForms with IBM Cloud Pak​​ for Multicloud Management
{: #integrate-cloudforms-cp4mcm}

Enable navigation to CloudForms within the IBM Cloud Pak® console.

Complete the following steps on a Linux system. You can use the boot node from the HUB cluster where IBM Cloud Pak​​ for Multicloud Management is installed. These steps enable navigation to CloudForms from the IBM Cloud Pak​​ console:

1. Obtain the Automation navigation for IBM Cloud Pak​​ for Multicloud Management 1.3 script, `automation-navigation-updates.sh`, from [IBM Passport Advantage®](https://www.ibm.com/software/passportadvantage/) website. This script was downloaded from IBM Passport Advantage in the "Before you begin" section.

2. Install and authenticate `kubectl`. For more information, see [Installing the Kubernetes CLI (kubectl)](https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/kubectl/install_kubectl.html).

3. Install JQ. For more information, see [Download jq](https://stedolan.github.io/jq/download/).

4. Copy the `automation-navigation-updates.sh` script to a directory location. Set the file permissions on the script and run the script to enable navigation to your CloudForms instance:

   ```
   chmod 755 ./automation-navigation-updates.sh

   ./automation-navigation-updates.sh -c <CloudForms URL>
   ```
     
   * `-c` Is a required parameter that refers to the URL for the CloudForms console. For example, `https://vm17-cf-test.ibm.com/#/`

5. Verify that the CloudForms instance is in the IBM Cloud Pak​​ console navigation menu. From the IBM Cloud Pak​​ navigation menu, click **Automate infrastructure** > **CloudForms**.

CloudForms is integrated with the IBM Cloud Pak​​ console.

![Figure showing CloudForms integration in IBM Cloud Pak console.](images/results_access_CF.png){: caption="Figure 18. CloudForms integration in IBM Cloud Pak console" caption-side="bottom"}


## Step D. Enable Single Sign-on with CloudForms and IBM Cloud Pak​​ for Multicloud Management
{: #sso-cloudforms-cp4mcm}

CloudForms enables single sign-on integration with an enterprise identity provider through use of the OpenID Connect (OIDC). 

### Prerequisites
Single sign-on with CloudForms and IBM Cloud Pak for Multicloud Management requires an LDAP server connection.
For more information about adding an LDAP connection, see: [Configuring LDAP connection](https://www.ibm.com/support/knowledgecenter/SSFC4F_1.3.0/iam/3.4.0/configure_ldap.html)

Complete the single sign-on integration between IBM Cloud Pak​​ for Multicloud Management and CloudForms by completing these steps:
1. Register the CloudForms OIDC client with IAM. These steps are completed on the IBM Cloud Pak for Multicloud Management cluster.
2. Configure CloudForms to enable OIDC authentication with the same identity provider used for IBM Cloud Pak for Multicloud Management. These steps are completed on the CloudForms appliance.

### Step 1. Register CloudForms instance with IAM as an OIDC client 
{: #register-cf-with-IAM-as-OIDC-client}

In order to enable single sign-on (SSO) between IBM Cloud Pak​​ for Multicloud Management and CloudForms using OIDC, the CloudForms instance needs to register as an OIDC client with Identity and Access Management (IAM). Complete these steps on the IBM Cloud Pak for Multicloud Management cluster.

You can register CloudForms as an OIDC client with IAM using the `cloudctl` command.

The registration method requires the following registration payload in a file "registration.json":

```
{
  "token_endpoint_auth_method":"client_secret_basic",
  "client_id": "<CLIENT_ID>",
  "client_secret": "<CLIENT_SECRET>",
  "scope":"openid profile email",
  "grant_types":[
     "authorization_code",
     "client_credentials",
     "password",
     "implicit",
     "refresh_token",
     "urn:ietf:params:oauth:grant-type:jwt-bearer"
  ],
  "response_types":[
     "code",
     "token",
     "id_token token"
  ],
  "application_type":"web",
  "subject_type":"public",
  "post_logout_redirect_uris":[
     "https://<CP4MCM_CONSOLE_URL>"   ],
  "preauthorized_scope":"openid profile email general",
  "introspect_tokens":true,
  "trusted_uri_prefixes":[
     "https://<CP4MCM_CONSOLE_URL>/"    ],
  "redirect_uris":["https://<CP4MCM_CONSOLE_URL>/auth/liberty/callback","https://<CLOUDFORMS_URL>/oidc_login/redirect_uri"]
  }
  ```
  {: codeblock}

  Example registration payload (for reference only):
  ```
  {
 "token_endpoint_auth_method":"client_secret_basic",
 "client_id": "N3NzNVFsSjlLVkl6Zk5hZ01MRzJVaVdnbFcxNGl5cnQK",
 "client_secret": "VWNVNzF4ZUxNSVBQUHZHdG1xQmNsTTFOWmNUUGlnYUkK",
 "scope":"openid profile email",
 "grant_types":[
    "authorization_code",
    "client_credentials",
    "password",
    "implicit",
    "refresh_token",
    "urn:ietf:params:oauth:grant-type:jwt-bearer"
 ],
 "response_types":[
    "code",
    "token",
    "id_token token"
 ],
 "application_type":"web",
 "subject_type":"public",
 "post_logout_redirect_uris":["https://icp-console.apps.test.ibm.com"],
 "preauthorized_scope":"openid profile email general",
 "introspect_tokens":true,
 "trusted_uri_prefixes":["https://icp-console.apps.test.ibm.com/"],
 "redirect_uris":["https://icp-console.apps.test.ibm.com/auth/liberty/callback","https://www.cf-dev.test.ibm.com/oidc_login/redirect_uri"]
 }
 ```

1. Create a file named: 'registration.json` based on the example template. Replace the values in the example template payload registration with the actual values based on your installation. 

    - `CLIENT_ID` Your base64 encoded character string.
    - `CLIENT_SECRET` Your base64 encoded character string.
      
      **Note:** The `<CLIENT_ID>` and `<CLIENT_SECRET>` need to be generated. The values can be any string, but normally a 32 character string that is base64 encoded is used. You can use BASE64 to encode your character string. For more information, see: [BASE64](https://www.base64encode.org/). 
  
      Example command that uses base64 to encode a character string:
      ```
      #
      # Generate two encrypted streams from some longer-than-32-characters strings
      #
      echo There is a huge white elephant in LA zoo |base64
      echo 12345678901234567890123456789012345 |base64
      ```

    - `CP4MCM_CONSOLE_URL` The URL of the IBM Cloud Pak for Multicloud Management console.
    - `post_logout_redirect_uris` The URL of the IBM Cloud Pak for Multicloud Management console.
    - `trusted_uri_prefixes` The URL of the IBM Cloud Pak for Multicloud Management console with "forward slash" /.
    - `redirect_uris` The URL of the IBM Cloud Pak for Multicloud Management console with the path to callback and the URL of the CloudForms host with the path to the redirect_uri.

    **Note:** You can run the following command on the IBM Cloud Pak for Multicloud Management cluster to determine the URL of the IBM Cloud Pak for Multicloud Management console:
    ```
    oc get routes icp-console -o=jsonpath='{.spec.host}' -n kube-system
    ```

2. After the file 'registration.json' is completed, run the command to register CloudForms as an OIDC client.

    Example `cloudctl` command:
    ```
    cloudctl iam oauth-client-register -f registration.json
    ```
    {: codeblock}

    **Note:** If you receive an error from the `oauth-client-register` command, you can log back in by following these steps:
     1. Log back in to IBM cloud, https://cloud.ibm.com/
     2. Click the user icon in top right-hand corner
     3. Click **'Login to CLI and API'**
     4. Run the provided `ibmcloud login ...` CLI command to login to IBM Cloud
     5. Log in to the OpenShift web console
     6. Click **IAM#<yourID>** dropdown menu, then select "Copy Login Command"
     7. Click on Display Token
     8. Run the provided `oc login ...` CLI command to login to OpenShift console
     Login to IBM Cloud Pak for Multicloud Management:
     9. `oc get route -n kube-system`
     10. `cloudctl login -a https:<icp-console HOST/PORT>` **-n kube-system**
    
    **Important**:  Be sure to include the "**-n kube-system**" argument to specify this namespace, or else the `cloudctl iam` command can fail.


### Step 2. Configure CloudForms OIDC client to enable single sign on (SSO) 
{: #enable-single-sign-on}

CloudForms provides support for single sign-on integration with an enterprise identity provider through use of the OpenID Connect (OIDC).

Complete the configuration of single sign-on between IBM Cloud Pak​​ for Multicloud Management and CloudForms by following these steps.

### Import the Root CA certificate to CloudForms from IBM Cloud Pak​​ for Multicloud Management

1. Retrieve the cluster ca cert from IBM Cloud Pak​​ for Multicloud Management by running this command on the cluster:

  ```
  oc get secret -n kube-public ibmcloud-cluster-ca-cert -o jsonpath='{.data.ca\.crt}'| base64 --decode
  ```
  {: codeblock}

2. Copy and paste the output to a file, for example `ibm_cp_cf.crt`

3. Edit the file, `ibm_cp_cf.crt` and change:
   - `BEGIN CERTIFICATE` to `BEGIN TRUSTED CERTIFICATE`
   - `END CERTIFICATE` to `END TRUSTED CERTIFICATE`

    **Note:** The following steps should be completed by logging in to the CloudForms appliance system as root user:

4. Copy the updated `ibm_cp_cf.crt` file to the CloudForms appliance and save it in the directory: `/etc/pki/ca-trust/source/anchors`

5. Run the command:

  ```
  update-ca-trust
  ```
  {: codeblock}

6. Restart the evm server by running the command:

   ```
   systemctl restart evmserverd
   ```
   {: codeblock}

### Apache Configuration

**Note:** Complete these steps by logging in to the CloudForms appliance as root user:

Copy the Apache OIDC template configuration file:
```
#TEMPLATE_DIR="/opt/rh/cfme-appliance/TEMPLATE"
# cp ${TEMPLATE_DIR}/etc/httpd/conf.d/manageiq-remote-user-openidc.conf \
    /etc/httpd/conf.d/
# cp ${TEMPLATE_DIR}/etc/httpd/conf.d/manageiq-external-auth-openidc.conf.erb \
    /etc/httpd/conf.d/manageiq-external-auth-openidc.conf
```

### OIDC Configuration

The Apache `/etc/httpd/conf.d/manageiq-external-auth-openidc.conf` configuration file must be updated with installation-specific values. 
Replace the contents of the file with the actual values based on the installation. 

Example template for the configuration file:
```
LoadModule          auth_openidc_module modules/mod_auth_openidc.so
ServerName          https://<CF_HOSTNAME>

OIDCCLientID                  <CLIENT_ID>
OIDCClientSecret              <CLIENT_SECRET>  
OIDCRedirectURI                https://<CF_HOSTNAME>/oidc_login/redirect_uri
OIDCCryptoPassphrase           <passphrase>
OIDCOAuthRemoteUserClaim       sub
OIDCRemoteUserClaim            name

OIDCProviderIssuer                  https://127.0.0.1:443/idauth/oidc/endpoint/OP
OIDCProviderAuthorizationEndpoint   https://<CP4MCM_CONSOLE_URL>/idprovider/v1/auth/authorize
OIDCProviderTokenEndpoint           https://<CP4MCM_CONSOLE_URL>/idprovider/v1/auth/token
OIDCOAuthIntrospectionEndpoint      https://<CP4MCM_CONSOLE_URL>/idprovider/v1/auth/introspect
OIDCProviderJwksUri                 https://<CP4MCM_CONSOLE_URL>/oidc/endpoint/OP/jwk
OIDCProviderEndSessionEndpoint      https://<CP4MCM_CONSOLE_URL>/idprovider/v1/auth/logout

OIDCScope                        "openid email profile"
OIDCResponseMode                 "query"
OIDCProviderTokenEndpointAuth     client_secret_post

OIDCPassUserInfoAs json
OIDCSSLValidateServer off
OIDCHTTPTimeoutShort 10

<Location /oidc_login>
  AuthType  openid-connect
  Require   valid-user
  LogLevel   warn
</Location>
```
- `CF_HOSTNAME` Specifies the hostname of the CloudForms server.
- `CLIENT_ID` The client ID used for registering CloudForms as an OIDC client with IAM.
- `CLIENT_SECRET` The client ID used for registering CloudForms as an OIDC client with IAM.
- `CP4MCM_CONSOLE_URL` The URL of the IBM Cloud Pak for Multicloud Management console.
- `OIDCCryptoPassphrase` Can be any arbitrary alpha-numeric string.
- **Note:** The `CLIENT_ID` and `CLIENT_SECRET` values are generated when you register CloudForms as an OIDC client, see: [Register CloudForms instance with IAM as an OIDC client](#register-cloudforms-instance-with-iam-as-an-oidc-client).

Restart Apache on the CloudForms appliance as follows:
```
# systemctl restart httpd
```
{: codeblock}

### Configuring the Administrative UI

Update the Appliance Administrative UI to be OIDC aware and function. Complete these steps on each UI-enabled CloudForms appliance.

1. Log in as `admin`, then select the **Configuration** by clicking the gear icon.

2. Select the **Settings**, then select the **Authentication** tab.

3. In the **Authentication** section, set the **Mode** to `External (httpd)`

4. In the **External Authentication (HTTPd) Settings** section, set **Provider Type** to `Enable OpenID-Connect`. 
     - **Note:** This setting enables the OIDC login button on the login screen that redirects to the OIDC protected page for authentication, and supports the OIDC logout process.

5. Optional: In the **External Authentication (HTTPd) Settings** section, select **Enable Single Sign-On**.
     - **Note:** If you select this option, the initial access to the Appliance Administrative UI will redirect to the OIDC Identity Provider authentication screen.

6. In the **Role Settings** section, select the **Get User Groups from External Authentication (HTTPd)** setting.

7. Click Save.
     
8. Select **Access Control** and make sure the user’s groups are created on the Appliance and appropriate roles are assigned to those groups. The user's groups to be added in CloudForms should have the same names as the groups defined in the LDAP server that is configured in the IBM Cloud Pak console. 

    **Note:** Access control in CloudForms is based on group membership as roles are assigned to groups. When CloudForms is integrated with IBM Cloud Pak for Multicloud Management with SSO, it looks at the user’s group membership in the identity token and checks if that group exists in CloudForms. If the group doesn't exist, then access is denied. 
    
    **Important:** You must create groups in CloudForms that match your existing LDAP groups by name, and assign the groups account roles. At least one group to which the user belongs in LDAP that IBM Cloud Pak for Multicloud Management is configured to use must also be created in CloudForms. You must assign a proper role to this group in CloudForms. For more information, see: [CloudForms Roles](https://access.redhat.com/documentation/en-us/red_hat_cloudforms/5.0/html-single/general_configuration/index#roles).
    
    The LDAP configuration searches the UID and email attributes. Make sure that all accounts have a defined email attribute.

    Example:
    In LDAP a group that is named `group100` exists and a user with username `user100` is a member of the group. The user `user100` must have an email attribute defined and the group `group100` must be created in CloudForms.

9. Click Save.

### Congratulations!
You successfully installed and configured CloudForms and integrated CloudForms with IBM Cloud Pak for Multicloud Management in IBM Cloud.
