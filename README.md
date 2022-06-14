# aws-csp-policy-update
![Github Action](https://flat.badgen.net/badge/Github/Action/green?icon=github)
![MIT license](https://flat.badgen.net/badge/License/MIT/green)

Github action that Update the selected Custom Security headers policy with A New Version Policy JSON.

This allows a Self service approach for organization users to Update CloudFront CSP (Content-security-policy) in a more managed way.
1. The New CSP is published in a JSON file.
2. A PR is opened to the Cloud-Owner/Security-Owner.
3. Once the PR is approved the Action updates the policy automaticly.

## Usage
This action rely on the connection to AWS to be already established, this can be done by setting manually the environment variables: 
"AWS_ACCESS_KEY_ID", "AWS_SECRET_ACCESS_KEY", "AWS_DEFAULT_REGION". 
Or by the Using AssumeRole Action prior to running this Action. (As in the example below)

## Inputs
| Name | Description | Required |
| ---- | ----------- | -------- |
| response-header-policy-id | The ID of the policy you want to update  | yes |
| new-policy-file | Path to the New Policy JSON file | yes |


## Examples
```yaml
# Using Assume Role
jobs:
  updatepolicy:
    name: Update the CSP Policy
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: configure aws credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          role-to-assume: ${{ secrets.ASSUME_ROLE_ARN }}
          role-session-name: aws_session
          aws-region: us-east-1
      
      - name: CloudFront CSP Update
        uses: deftdot/aws-csp-policy-update@v1
        with:
          response-header-policy-id: ${{ secrets.POLICY_ID }}
          new-policy-file: 'policies/policy.json' #Assume the policy JSON located at {repo_root}/policies/policy.json
```

```yaml
# Using AWS Credentials in secrets
jobs:
  updatepolicy:
    name: Update the CSP Policy
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}
      
      - name: CloudFront CSP Update
        uses: deftdot/aws-csp-policy-update@v1
        with:
          response-header-policy-id: ${{ secrets.POLICY_ID }}
          new-policy-file: 'policies/policy.json' #Assume the policy JSON located at {repo_root}/policies/policy.json
```
