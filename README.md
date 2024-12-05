# Wiz AWS Connector Management Pipeline

This repository contains a GitHub Actions pipeline to automate the management of the Wiz AWS Connector, ensuring that the CloudFormation (CFN) templates remain up-to-date and deployed across your AWS organization. 

## Features
- Automatically checks for updates to the Wiz AWS CFN template on a weekly basis or via manual triggers.
- Compares the current template with the latest version and creates a pull request if changes are detected.
- Automates the update of the Wiz CloudFormation StackSet once changes are approved and merged.
- Provides manual steps for initial setup.

## Prerequisites
Before using this pipeline, ensure the following prerequisites are in place:

1. **OpenID Connector and AWS Role**: 
   - An OpenID connector must be created.
   - A corresponding AWS IAM role must be configured to allow this pipeline to assume necessary permissions.
   
2. **Initial CloudFormation StackSet Deployment**:
   - The Wiz CloudFormation StackSet must be manually deployed initially to set up the connector. Refer to the official [Wiz documentation](https://www.wiz.io) for guidance.

3. **Required Commands**:
   Placeholder for required CLI commands:
   - `aws iam create-open-id-connect-provider --url "https://token.actions.githubusercontent.com" --thumbprint-list "6938fd4d98bab03faadb97b34396831e3780aea1" --client-id-list 'sts.amazonaws.com'` 
   - `aws iam create-role --role-name github-wiz-pipeline-role --assume-role-policy-document file://trust-policy.json` (file in repo)
   - `aws iam attach-role-policy --role-name github-wiz-pipeline-role --policy-arn arn:aws:iam::aws:policy/AWSCloudFormationFullAccess`
   - 'aws iam attach-role-policy --role-name github-wiz-pipeline-role --policy-arn arn:aws:iam::aws:policy/IAMFullAccess'

## How It Works
### Workflow Triggers
- **Push to `main` Branch**: Updates the StackSet automatically when changes are merged into the `main` branch.
- **Scheduled Run**: Checks for updates to the Wiz CFN template weekly, on Sunday at midnight (UTC).
- **Manual Trigger**: The workflow can also be run manually if needed.

### Pipeline Steps
1. **Check for Updates**: Downloads the latest CFN template and compares it with the existing one in the repository.
2. **Create Pull Request**: If changes are detected, a new branch is created, and a pull request is opened for review.
3. **Update StackSet**: Once changes are merged into `main`, the pipeline updates the Wiz CloudFormation StackSet in AWS.
4. **Change Set Execution**: Applies the changes to the Wiz StackSet.

## Manual Steps
- **Initial StackSet Deployment**: 
  Deploy the initial CloudFormation StackSet using the AWS Management Console or CLI.
