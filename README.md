# LinearB Deployment Detection Action

A GitHub Action to report your deployments to LinearB for tracking deployment activity and metrics.

## Overview

This action helps you integrate your deployment workflow with LinearB, allowing you to:

- Track deployment frequency
- Measure deployment lead time
- Monitor deployment success rates
- Connect deployments to your code changes

## Usage

Add this action to your GitHub workflow:

```yaml
- name: Report Deployment to LinearB
  uses: joehutchinson/linearb-deployment@v1
  with:
    repo_url: ${{ github.server_url }}/${{ github.repository }}
    api_token: ${{ secrets.LINEARB_API_TOKEN }}
    ref_name: ${{ github.sha }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `repo_url` | The git repository URL (must match the pattern `^https://[a-zA-Z0-9./+^@_%-]{15,250}$`) | Yes | N/A |
| `api_token` | LinearB API token | Yes | N/A |
| `ref_name` | Ref name of the release, accepts any Git ref (i.e. commit short or long SHA/tag name) | Yes | N/A |
| `timestamp` | The deployment or custom pre-deployment stage timestamp | No | Current time |
| `stage` | The key of the custom pre-deployment stage | No | N/A |
| `services` | List of LinearB services names monitoring this | No | N/A |

## Requirements

- A LinearB account with API access
- An API token with appropriate permissions

## Security Best Practices

When using this action, follow these security best practices:

1. **Never hardcode your API token** in your workflow files or commit it to your repository
2. **Always use GitHub Secrets** to store your LinearB API token:
   ```yaml
   # In your repository settings, add LINEARB_API_TOKEN as a secret
   # Then reference it in your workflow:
   with:
     api_token: ${{ secrets.LINEARB_API_TOKEN }}
   ```
3. **Limit secret access** by setting appropriate permissions on your repository and workflows
4. **Regularly rotate your API tokens** as part of your security practices

## Example Workflow

```yaml
name: Deploy and Report

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      # Your deployment steps here
      
      - name: Report to LinearB
        uses: joehutchinson/linearb-deployment@v1
        with:
          repo_url: ${{ github.server_url }}/${{ github.repository }}
          api_token: ${{ secrets.LINEARB_API_TOKEN }}
          ref_name: ${{ github.sha }}
          services: '["service-name-1", "service-name-2"]'
```

## Notes

- The `repo_url` must match the regex pattern: `^https://[a-zA-Z0-9./+^@_%-]{15,250}$`
- The action uses a composite run to execute the deployment reporting

## License

MIT
