# Salesforce Deploy GitHub Action

This GitHub action is designed to create a new Salesforce Scratch org and deploy the provided packages to it. It assumes
that it's running in the `salesforce/cli:latest-full` container.

## Inputs

The action accepts the following inputs:

- `jwt_key`: Salesforce JWT Key (required)
- `username`: Salesforce Username (required)
- `client_id`: Salesforce Client ID (required)
- `instance_url`: Salesforce Dev Hub Instance URL (default: 'https://login.salesforce.com')
- `duration_days`: Duration in days for the scratch org (default: '7')
- `scratch_def`: Scratch org definition (default: JSON with "LightningSalesConsole" and "LightningServiceConsole" features)
- `api_version`: API Version
- `packages`: 04t or aliased packages to install
- `preview`: Create a preview scratch org (default: 'false')
- `org_name`: Name of the scratch org (required)
- `post_deploy`: A post deploy script written in Powershell

## Outputs

The action provides the following output:

- `url`: URL of the scratch org

## Usage

Here is an example of how to use this action in your workflow:

```yaml
- name: Salesforce Deploy
  uses: enosix/ghac-salesforce-deploy@stable
  with:
    jwt_key: ${{ secrets.JWT_KEY }}
    username: ${{ secrets.USERNAME }}
    client_id: ${{ secrets.CLIENT_ID }}
    duration_days: '7'
    preview: 'false'
    org_name: 'my-org-name'
    packages: |
        04t...
        04t...
```

This action will log in to Salesforce, create a new scratch org, install packages if provided, and add a comment if the
event is a pull request. The URL of the created scratch org will be available as an output of the action.

If the step fails on a non-pr acion, it will automatically attempt to retry the job three times. On PRs, a failure is not retried automatically.
