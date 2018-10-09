## Using Azure Keyvault to Store and Manage Secrets in Python Azure Functions

One issue with service lifecycle, including functions, is how to handle secrets.  One
solution is to inject the secrets into a particular script at time of deployment.  
Another is to use checked in configuration files with variations based upon deployment
type.  Yet another, if dealing with Docker based applications (or similar) is to 
inject values as environment variables -- this is usually the cleanest option.

However, with Azure Functions, the environment variables are a hard coded configuration
file which requires management.  Another option is to use a service that protects
secrets, like Azure Keyvault.

In fact, it is possible to configure access to and enable an Azure Function to use
Azure Keyvault through a service known as MSI or Managed Service Identity.  In order
to enable this, one must first:

- Create an Azure Keyvault
- Give your Azure Function application an identity
- Configure the Azure Function identity to have access to the Azure Keyvault

Azure Keyvault has granular access policies.  For this example we assume the 
function just requires read access to secrets.

Once the Keyvault is setup and configured, the function itself needs to be 
configured to know the location of the Keyvault as well as the names of the 
secrets to retrieve.  This still requires configuring the environment, but it
does not expose the secrets outside of Keyvault.

The script `setup_keyvault_and_app_permissions.sh` handles the three steps 
above.  In order to run it, it needs three commandline values:

- Resource Group Name --> `RESOURCE_GROUP_NAME`
- Keyvault Name --> `KEYVAULT_NAME`
- Application Name --> `AZURE_FUNC_APP_NAME`

For the example below, the values used for [this](../../setup/initial/README.md)
walkthrough are used, specifically for `RESOURCE_GROUP_NAME` and  `AZURE_FUNC_APP_NAME`.
For the Keyvault Name, we will use the value `jmsfunckv` as the name.

To launch the script:

```bash
export RESOURCE_GROUP_NAME=jmsazfunc1rg
export KEYVAULT_NAME=jmsfunckv
export AZURE_FUNC_APP_NAME=jmsazfunapp1
./setup_keyvault_and_app_permissions.sh $RESOURCE_GROUP_NAME $KEYVAULT_NAME $AZURE_FUNC_APP_NAME

```