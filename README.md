# Deploy to Azure Guide
Step-by-step guide showing how to configure GitHub to allow deployments to Azure.  The below steps are quick-start steps.  OpenID Connect offers a more secure path.  From GitHub's documentation:

*However, using hardcoded secrets requires you to create credentials in the cloud provider and then duplicate them in GitHub as a secret.

With OpenID Connect (OIDC), you can take a different approach by configuring your workflow to request a short-lived access token directly from the cloud provider. Your cloud provider also needs to support OIDC on their end, and you must configure a trust relationship that controls which workflows are able to request the access tokens. Providers that currently support OIDC include Amazon Web Services, Azure, Google Cloud Platform, and HashiCorp Vault, among others.*

# Configure permissions and GitHub secrets

### 1: Configure `GITHUB_TOKEN` permissions

At the start of each workflow run, GitHub automatically creates a unique `GITHUB_TOKEN` secret to use in your workflow. Make sure this token has the permissions required for this course.

1. Open a new browser tab, and work on the steps in your second tab while you read the instructions in this tab.
1. Go to Settings > Actions > General. Ensure that the `GITHUB_TOKEN` also has **Read and write permissions** enabled under **Workflow permissions**. This is required for your workflow to be able to upload your image to the container registry.

### 2: Store Azure credentials in GitHub secrets

1. Install Azure CLI
2. In terminal, run:
    ```shell
    az login
    ```
3. Copy the value of the id: field and create a new repository secret called AZURE_SUBSCRIPTION_ID. To do this, click on the repository's **Secrets and variables > Actions** in the Settings tab.
4. In terminal, run:
    ````shell
    az ad sp create-for-rbac --name "GitHub-Actions" --role contributor \
     --scopes /subscriptions/{subscription-id} \
     --sdk-auth

        # Replace {subscription-id} with the same id stored in AZURE_SUBSCRIPTION_ID.
        ```

    > **Note**: The `\` character works as a line break on Unix based systems. If you are on a Windows based system the `\` character will cause this command to fail. Place this command on a single line if you are using Windows.\*\*

    ````
5. Copy the entire contents of the command's response; create a new repository secret called `AZURE_CREDENTIALS`. To do this, click on the repository's **Secrets and variables > Actions** in the Settings tab.
