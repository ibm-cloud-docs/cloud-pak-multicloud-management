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

# Getting started deploying Infrastructure management as a containerized deployment (podified).

Complete these steps to install Infrastructure management as a containerized deployment.
{: shortdesc}

## Prerequisites
{: #pre-requisites}
- You must have {{site.data.keyword.cp4mcm_full_notm}} V2.1 installed. For more information, see [Getting started with {{site.data.keyword.cp4mcm_full_notm}} V2.1](https://test.cloud.ibm.com/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-getting-started-21)  

- Ensure you enable the operators for `Infrastructure management` by opening the {{site.data.keyword.cp4mcm_full_notm}} installation YAML file. Locate the `pakModules` section, and change `enabled: false` to `enabled: true`. Enable these Infrastructure management-related operators:

  - `ibm-management-im-install` for Infrastructure management. For more information, see [Infrastructure management](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/mcm/infrastructure/infra_mgmt_intro.html).

  - `ibm-management-infra-vm` for provisioning virtual machines and instances. For more information, see [Provisioning Virtual Machines and Instances](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/Infra_mgmt/provisioning_virtual_machines_and_hosts/index.html).

  - `ibm-management-infra-grc` for Infrastructure management. For more information, see [Governance, risk, and compliance](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/mcm/compliance/compliance_intro.html).

  For more information, see [Enabling operators after IBM Cloud Pak for Multicloud Management installation](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/install/enable_operator.html).

- You must configure and connect an LDAP directory with {{site.data.keyword.cp4mcm_full_notm}}. You must have an LDAP group in your configuration for {{site.data.keyword.cp4mcm_full_notm}} with users defined who will access Infrastructure management. For more information, see the [IAM Guide](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/iam/3.x.x/iam_guide.html).

- ***cloudctl*** must be installed and authenticated to your cluster. You must install the IBM Cloud Pak CLI, `cloudctl`. For more information, see [Installing the IBM Cloud Pak CLI ![Opens in a new tab](../../images/icons/launch-glyph.svg "Opens in a new tab")](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/cloudctl/install_cli.html).

  **Note:** Download the installation file for CLI tools from the console.

- ***oc*** must be installed and authenticated to your cluster. If OpenShift Container Platform CLI tools are not installed on the system, you need to download, decompress, and install the OpenShift Container Platform CLI tools `oc` from [OpenShift Container Platform client binary files](https://mirror.openshift.com/pub/openshift-v4/clients/).

## Step 1. Generate client ID and client secret
{: #gen-client-id-and-client-secret}

Create unique strings to use for **client ID** and **client secret**. You can generate base64-encoded 32 character strings. Examples:

    $ echo There is a huge white elephant in LA zoo | base64 > client_id.txt
    $ echo 12345678901234567890123456789012345 | base64 > client_secret.txt

Retain these values as you need them for subsequent steps. You can use the examples and the values are available in the files client_id.txt and client_secret.txt for later reference.

**Note:** These values in subsequent steps are referenced as `<YOUR_CLIENT_ID>` and `<YOUR_CLIENT_SECRET>`.

## Step 2. Determine {{site.data.keyword.cp4mcm_full_notm}} Route
{: #cp4mcm-route}

Determine the main console URL for {{site.data.keyword.cp4mcm_full_notm}} by using the cp-console route. Run the following command:

    $ oc -n ibm-common-services get route cp-console --template '{{.spec.host}}'

Example command output: `cp-console.apps.mycluster.os.fyre.ibm.com`

**Note:** This value in subsequent steps is referenced as `<YOUR_CP4MCM_ROUTE>`.

## Step 3. Determine Infrastructure management Route
{: #infra-mgmt-route}

Determine the main URL (HTTPd route) of the Infrastructure management installation. Since Infrastructure management is not yet installed, the route is an *assumed* route as the eventual route doesn't exist yet. 

This can be derived by appending the Infrastructure management installation name (default: `inframgmtinstall`) to the cluster domain.

Example: `inframgmtinstall.apps.mycluster.os.fyre.ibm.com`

**Note:** This value in subsequent steps is referenced as `<YOUR_IM_HTTPD_ROUTE>`. Specify host names using lowercase letters only. Uppercase letters and underscore (_) characters are not permitted in host names.

## Step 4. Register Infrastructure management with IAM as an OIDC client
{: #register-im-with-iam}

Complete these steps on the {{site.data.keyword.cp4mcm_full_notm}} cluster. Create and update the example `registration.json` file with the values specific to your cluster. Replace the values in brackets (<>) with your specific values, which are derived from the previous steps.

You can register Infrastructure management as an OIDC client with IAM (Common Services) by using the `cloudctl` command.

The registration method requires the following registration payload in a file `registration.json`:

```
{
	"token_endpoint_auth_method": "client_secret_basic",
	"client_id": "<YOUR_CLIENT_ID>",
	"client_secret": "<YOUR_CLIENT_SECRET>",
	"scope": "openid profile email",
	"grant_types": [
		"authorization_code",
		"client_credentials",
		"password",
		"implicit",
		"refresh_token",
		"urn:ietf:params:oauth:grant-type:jwt-bearer"
	],
	"response_types": [
		"code",
		"token",
		"id_token token"
	],
	"application_type": "web",
	"subject_type": "public",
	"post_logout_redirect_uris": [
		"https://<YOUR_CP4MCM_ROUTE>"
	],
	"preauthorized_scope": "openid profile email general",
	"introspect_tokens": true,
	"trusted_uri_prefixes": [
		"https://<YOUR_CP4MCM_ROUTE>/"
	],
	"redirect_uris": ["https://<YOUR_CP4MCM_ROUTE>/auth/liberty/callback", "https://<YOUR_IM_HTTPD_ROUTE>/oidc_login/redirect_uri"]
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
	"post_logout_redirect_uris":["https://cp-console.apps.mycluster.os.fyre.ibm.com"],
	"preauthorized_scope":"openid profile email general",
	"introspect_tokens":true,
	"trusted_uri_prefixes":["https://cp-console.apps.mycluster.os.fyre.ibm.com/"],
	"redirect_uris":["https://cp-console.apps.mycluster.os.fyre.ibm.com/auth/liberty/callback","https://inframgmtinstall.apps.mycluster.os.fyre.ibm.com/oidc_login/redirect_uri"]
}
```

1. Create a file named: `registration.json` based on the example template. Replace the values in the example template payload registration with the actual values based on your installation. 

    - `<YOUR_CLIENT_ID>` Your base64 encoded character string.
    - `<YOUR_CLIENT_SECRET>` Your base64 encoded character string.
      
      **Note:** The `<YOUR_CLIENT_ID>` and `<YOUR_CLIENT_SECRET>` were generated in [Generate client ID and client secret](#gen-client-id-and-client-secret). 
    - `<YOUR_CP4MCM_ROUTE>` The URL of the IBM Cloud Pak for Multicloud Management console. See [Determine {{site.data.keyword.cp4mcm_full_notm}} Route](#cp4mcm-route).
    - `<YOUR_IM_HTTPD_ROUTE>` The main URL of the Infrastructure management installation. See [Determine Infrastructure management Route](#infra-mgmt-route).
     
    **Note:**
      Since Infrastructure management is not yet installed, the route `<YOUR_IM_HTTPD_ROUTE>` is an *assumed* route as the eventual route doesn't exist yet. This can be determined by appending the {{site.data.keyword.cp4mcm_full_notm}} installation name (default: `inframgmtinstall`) to the cluster domain. Example: `inframgmtinstall.apps.mycluster.os.fyre.ibm.com`    
    
2. After the file `registration.json` is completed, log in and run the command to register Infrastructure management as an OIDC client.
   
   **Note:** Include the `-n ibm-common-services` to specify this project, or the `cloudctl iam` command can fail.
    ```
    cloudctl login -a https://<YOUR_CP4MCM_ROUTE> -n ibm-common-services
    ```
    {: codeblock}

    Example `cloudctl iam` command:
    ```
    cloudctl iam oauth-client-register -f registration.json
    ```
    {: codeblock}

  Next, create a secret that is used by the Infrastructure management installation operator.

## Step 5. Create Secret for Infrastructure management OIDC configuration
{: #create-secret}

Create a secret that is used by the Infrastructure management installation operator when an Infrastructure management instance is installed.

Create a `im-oidc-secret.yaml` file with the values specific to your system, which are derived from the previous steps. 

```yaml
kind: Secret                                                                        
apiVersion: v1                                                                            
metadata:                                                                     
  name: imconnectionsecret                                                          
stringData:
  oidc.conf: |-                       
    LoadModule                          auth_openidc_module modules/mod_auth_openidc.so
    ServerName                          https://<YOUR_IM_HTTPD_ROUTE>
    LogLevel                            debug
    OIDCCLientID                        <YOUR_CLIENT_ID>
    OIDCClientSecret                    <YOUR_CLIENT_SECRET>
    OIDCRedirectURI                     https://<YOUR_IM_HTTPD_ROUTE>/oidc_login/redirect_uri
    OIDCCryptoPassphrase                alphabeta
    OIDCOAuthRemoteUserClaim            sub
    OIDCRemoteUserClaim                 name
    # OIDCProviderMetadataURL missing
    OIDCProviderIssuer                  https://127.0.0.1:443/idauth/oidc/endpoint/OP
    OIDCProviderAuthorizationEndpoint   https://<YOUR_CP4MCM_ROUTE>/idprovider/v1/auth/authorize
    OIDCProviderTokenEndpoint           https://<YOUR_CP4MCM_ROUTE>/idprovider/v1/auth/token
    OIDCOAuthCLientID                   <YOUR_CLIENT_ID>
    OIDCOAuthClientSecret               <YOUR_CLIENT_SECRET>
    OIDCOAuthIntrospectionEndpoint      https://<YOUR_CP4MCM_ROUTE>/idprovider/v1/auth/introspect
    # ? OIDCOAuthVerifyJwksUri          https://<YOUR_CP4MCM_ROUTE>/oidc/endpoint/OP/jwk
    OIDCProviderJwksUri                 https://<YOUR_CP4MCM_ROUTE>/oidc/endpoint/OP/jwk
    OIDCProviderEndSessionEndpoint      https://<YOUR_CP4MCM_ROUTE>/idprovider/v1/auth/logout
    OIDCScope                           "openid email profile"
    OIDCResponseMode                    "query"
    OIDCProviderTokenEndpointAuth       client_secret_post
    OIDCOAuthIntrospectionEndpointAuth  client_secret_basic
    OIDCPassUserInfoAs                  json
    OIDCSSLValidateServer               off
    OIDCHTTPTimeoutShort                10
    OIDCCacheEncrypt                    On
    
    <Location /oidc_login>
      AuthType  openid-connect
      Require   valid-user
      LogLevel   debug
    </Location>
    <Location /ui/service/oidc_login>
      AuthType          openid-connect
      Require          valid-user
      Header set Set-Cookie   "miq_oidc_access_token=%{OIDC_access_token}e; Max-Age=10; Path=/ui/service"
    </Location>  
    <LocationMatch ^/api(?!\/(v[\d\.]+\/)?product_info$)>
      SetEnvIf Authorization '^Basic +YWRtaW46'     let_admin_in
      SetEnvIf X-Auth-Token  '^.+$'                 let_api_token_in
      SetEnvIf X-MIQ-Token   '^.+$'                 let_sys_token_in
      SetEnvIf X-CSRF-Token  '^.+$'                 let_csrf_token_in
      AuthType     oauth20
      AuthName     "External Authentication (oauth20) for API"
      Require   valid-user
      Order          Allow,Deny
      Allow from env=let_admin_in
      Allow from env=let_api_token_in
      Allow from env=let_sys_token_in
      Allow from env=let_csrf_token_in
      Satisfy Any
      LogLevel   debug
    </LocationMatch>
    OIDCSSLValidateServer      Off
    OIDCOAuthSSLValidateServer Off
    RequestHeader unset X_REMOTE_USER                                                                            
    RequestHeader set X_REMOTE_USER           %{OIDC_CLAIM_PREFERRED_USERNAME}e env=OIDC_CLAIM_PREFERRED_USERNAME
    RequestHeader set X_EXTERNAL_AUTH_ERROR   %{EXTERNAL_AUTH_ERROR}e           env=EXTERNAL_AUTH_ERROR          
    RequestHeader set X_REMOTE_USER_EMAIL     %{OIDC_CLAIM_EMAIL}e              env=OIDC_CLAIM_EMAIL             
    RequestHeader set X_REMOTE_USER_FIRSTNAME %{OIDC_CLAIM_GIVEN_NAME}e         env=OIDC_CLAIM_GIVEN_NAME        
    RequestHeader set X_REMOTE_USER_LASTNAME  %{OIDC_CLAIM_FAMILY_NAME}e        env=OIDC_CLAIM_FAMILY_NAME       
    RequestHeader set X_REMOTE_USER_FULLNAME  %{OIDC_CLAIM_NAME}e               env=OIDC_CLAIM_NAME              
    RequestHeader set X_REMOTE_USER_GROUPS    %{OIDC_CLAIM_GROUPS}e             env=OIDC_CLAIM_GROUPS            
    RequestHeader set X_REMOTE_USER_DOMAIN    %{OIDC_CLAIM_DOMAIN}e             env=OIDC_CLAIM_DOMAIN 
  ```
  {: codeblock}

1. Create a file named: `im-oidc-secret.yaml` based on the example template. Replace the values in the example template with the actual values based on your installation. 
 
    **Note:** Use the same values from the previous step for the `registration.json` file.
    - `<YOUR_CLIENT_ID>` Your base64 encoded character string.
    - `<YOUR_CLIENT_SECRET>` Your base64 encoded character string.
      
      **Note:** The `<YOUR_CLIENT_ID>` and `<YOUR_CLIENT_SECRET>` were generated in [Generate client ID and client secret](#gen-client-id-and-client-secret). 
    - `<YOUR_CP4MCM_ROUTE>` The URL of the IBM Cloud Pak for Multicloud Management console. See [Determine {{site.data.keyword.cp4mcm_full_notm}} Route](#cp4mcm-route).
    - `<YOUR_IM_HTTPD_ROUTE>` The main URL of the Infrastructure management installation. See [Determine Infrastructure management Route](#infra-mgmt-route).
     
      **Note:**

      Since Infrastructure management is not yet installed, the route `<YOUR_IM_HTTPD_ROUTE>` is an *assumed* route as the eventual route doesn't exist yet. This can be determined by appending the {{site.data.keyword.cp4mcm_full_notm}} installation name (default: `inframgmtinstall`) to the cluster domain. Example: `inframgmtinstall.apps.mycluster.os.fyre.ibm.com`  
    
2. Generate the secret using the `oc` command. The namespace is the same namespace of the Infrastructure management install operator, default: `management-infrastructure-management`

    ```
    oc create -n management-infrastructure-management -f im-oidc-secret.yaml
    ```
    {: codeblock}

The default name of the secret is `imconnectionsecret`. You can update the secret name in *im-oidc-secret.yaml* if wanted. 
    
## Step 6. Deploy the ibm-management-im-install operand
{: #deploy-install-operand}

After the OIDC configuration is completed for Infrastructure management, you can apply the CR to install an instance of Infrastructure management by using the Infrastructure management installation operator. You can install an instance of Infrastructure management by using the OpenShift console or by using the CLI (command-line tools).

* Specify the Infrastructure management route `<YOUR_IM_HTTPD_ROUTE>` in the CR field `applicationDomain`.
    *  applicationDomain: *inframgmtinstall.apps.mycluster.os.fyre.ibm.com*
    
    **Note:** Use the same value for `<YOUR_IM_HTTPD_ROUTE>` that you used in the OIDC configuration in the CR and operand deployments. `<YOUR_IM_HTTPD_ROUTE>` is the main URL of the Infrastructure management installation.
* If deploying as part of a multi-region setup, specify a unique number in the CR field `databaseRegion`.  If not specified, this value will default to 0.
    *  databaseRegion: *99*
* Add the following fields to enable OIDC. Reference the secret name created in the CR field `httpdAuthConfig`
    *  httpdAuthenticationType: openid-connect
    *  httpdAuthConfig: imconnectionsecret
    *  enableSSO: true
* Reference the default admin group in the CR field `initialAdminGroup`. Upon application of the CR, this group is created in Infrastructure management. Specify an existing group in your LDAP configuration.
    *  initialAdminGroupName: `<YOUR_LDAP_GROUP>`
    
    **Important:** This LDAP group is created in Infrastructure management to match your existing LDAP group by name, and assigns the group account roles. At least one group to which the users belong in LDAP that {{site.data.keyword.cp4mcm_full_notm}} is configured to use must be specified for `<YOUR_LDAP_GROUP>`. For example, you have an existing LDAP group that is named group100 and a user with the username user100 is a member of the group. You enter group100 for the value of `<YOUR_LDAP_GROUP>`.
* Add the image pull secret `imagePullSecret` to access the Infrastructure management images required by the installer.
    *  imagePullSecret: *my-pull-secret*
     
       **Note:** `imagePullSecret` is the same secret that was created for entitled registry. For more information, see [Create the entitled registry secret](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/install/prep_online.html#er)

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
Apply the CR by using the Red Hat® OpenShift console or the CLI.

### Install by using the OpenShift Console

1. Verify that the Infrastructure management operator is already installed.
2. Create an instance to deploy Infrastructure management.
   1. Browse to **Operators > Installed Operators**.
   2. Click **IBM Infrastructure Management Install**.
   3. Click **Create Instance**.
   4. Select **Update YAML** (see the example CR)
   5. Click **Create**
3. Verify that pods are running (it can take up to 15 minutes for pods to start).

### Install by using the CLI

1. Verify that the Infrastructure management operator is already installed.
2. Create a `infra.management.ibm.com_v1alpha1_iminstall_cr.yaml` file (see the example CR).
3. Update the YAML file.
4. Create an instance to deploy Infrastructure management by running the command:
    ~~~
    $ oc apply -f infra.management.ibm.com_v1alpha1_iminstall_cr.yaml
    ~~~
    {: codeblock}
5. Verify that pods are running (it can take up to 15 minutes for pods to start).

## Step 7. Create a secret for functional operators usage
{: #oper_secret}

To use the functional operators (`ibm-management-infra-vm`, and `ibm-management-infra-grc`) with Infrastructure management, you need a Connection CR. You can use the example `imconnection.yaml` file to create a default connection and update as needed.
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

## Step 8. Integrating Infrastructure management with the IBM Cloud Pak console
{: #auto_script}

Enable navigation to Infrastructure management with the IBM Cloud Pak console.

Download the part number: CC7X4EN
 
 | Description                                                                      | File name                               | Passport Advantage part number |
   |----------------------------------------------------------------------------------|-----------------------------------------|--------------------------------|
   | Automation navigation for IBM Cloud Pak® for Multicloud Management 2.1 | automation-navigation-updates.sh | CC7X4EN |

1. Obtain the menu customization script, `automation-navigation-updates.sh`, from [IBM Passport Advantage® ![Opens in a new tab](../../images/icons/launch-glyph.svg "Opens in a new tab")](https://www.ibm.com/software/passportadvantage/){: new_window} website. You must run the script on a Linux operating system. 

2. Install and authenticate `kubectl`. For more information, see [Managing your clusters with {{site.data.keyword.cp4mcm_full_notm}} ![Opens in a new tab](../../images/icons/launch-glyph.svg "Opens in a new tab")](https://www.ibm.com/support/knowledgecenter/SSFC4F_2.1.0/kubectl/install_kubectl.html).

3. Download and configure the JQ tool by using the following commands:
   ```
   wget -O jq https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64

   chmod +x ./jq

   mv jq /usr/bin
   ```
   For more information, see [Download jq ![Opens in a new tab](../../images/icons/launch-glyph.svg "Opens in a new tab")](https://stedolan.github.io/jq/download/){: new_window}.

4. Run the following commands to enable navigation to your Infrastructure management instance:

   ```
   chmod 755 ./automation-navigation-updates.sh

   ./automation-navigation-updates.sh -p management-infrastructure-management
   ```
   {: codeblock}

   * The `automation-navigation-updates.sh` expects the default namespace `management-infrastructure-management`. If you use a different namespace to install Infrastructure management, you must pass the namespace as a parameter when you run the command. Example:
   ```
   ./automation-navigation-updates.sh -p <namespace>
   ```
   {: codeblock}

5. Verify that the Infrastructure management instance is in the IBM Cloud Pak console navigation menu. From the IBM Cloud Pak for Multicloud Management menu, click **Automate infrastructure** > **Infrastructure management**.

