# Flexible EKS Deployment GitHub Actions Workflow

This repository contains a flexible GitHub Actions workflow for deploying applications to Amazon EKS (Elastic Kubernetes Service). The workflow automates the process of building a Docker image, pushing it to Amazon ECR (Elastic Container Registry), and deploying it to an EKS cluster.

## Features

- Configurable deployment branch
- Customizable AWS region, ECR repository, and EKS cluster name
- Smart Dockerfile detection
- Automatic deployment creation or update
- Configurable application port and Kubernetes namespace
- Secure handling of AWS credentials
- Flexible image tagging
- Customizable kubectl version
- Adaptable deployment step

## Prerequisites

Before using this workflow, ensure you have the following:

1. An AWS account with appropriate permissions for ECR and EKS
2. An EKS cluster set up in your AWS account
3. An ECR repository created in your AWS account
4. AWS CLI configured with the necessary credentials
5. kubectl installed and configured to interact with your EKS cluster
6. A Dockerfile in your repository

## Setup

1. Copy the workflow YAML file to your repository's `.github/workflows/` directory.
2. Set up the following secrets in your GitHub repository settings:
   - `AWS_ACCESS_KEY_ID`: Your AWS access key ID
   - `AWS_SECRET_ACCESS_KEY`: Your AWS secret access key

3. Customize the workflow by setting the following variables in your GitHub repository settings or directly in the workflow file:
   - `DEPLOY_BRANCH` (default: 'main'): The branch that triggers the deployment
   - `AWS_REGION` (default: 'us-west-2'): Your AWS region
   - `ECR_REPOSITORY` (default: 'my-app'): Your ECR repository name
   - `SERVICE_NAME` (default: 'MyService'): Your service name
   - `EKS_CLUSTER_NAME` (default: 'my-cluster'): Your EKS cluster name
   - `ENVIRONMENT` (default: 'production'): The deployment environment
   - `KUBECTL_VERSION` (default: 'latest'): The kubectl version to use
   - `DOCKERFILE_PATH` (optional): The path to your Dockerfile relative to the repository root
   - `APP_PORT` (default: '80'): The port your application listens on
   - `K8S_NAMESPACE` (default: 'default'): The Kubernetes namespace to deploy to

## Usage

Once set up, the workflow will automatically run when you push to the specified deployment branch. You can also manually trigger the workflow from the "Actions" tab in your GitHub repository.

## Customization

### Dockerfile Detection

The workflow uses a smart Dockerfile detection mechanism:

1. It first checks for the Dockerfile at the path specified by `DOCKERFILE_PATH` (if set).
2. If not found or not set, it looks for a file named `dockerfile` (lowercase) in the repository root.
3. If still not found, it looks for a file named `Dockerfile` (capitalized) in the repository root.
4. If no Dockerfile is found, the workflow will fail with an error message.

To use a Dockerfile in a non-standard location, set the `DOCKERFILE_PATH` variable to the relative path from the repository root.

### Deployment Behavior

The workflow checks if a deployment with the specified `SERVICE_NAME` already exists in the specified `K8S_NAMESPACE`:

- If the deployment doesn't exist, it creates a new deployment and a ClusterIP service.
- If the deployment exists, it updates the existing deployment with the new image.

### Application Port and Namespace

You can customize the application port and Kubernetes namespace by setting the `APP_PORT` and `K8S_NAMESPACE` variables, respectively. These are used when creating a new deployment and service.

### Additional Steps

You can add more steps to the workflow as needed, such as running tests, performing database migrations, or notifying external services.

## Troubleshooting

If you encounter issues with the workflow:

1. Check the workflow run logs in the GitHub Actions tab of your repository.
2. Ensure all required secrets and variables are correctly set.
3. Verify that your AWS credentials have the necessary permissions.
4. Check that your EKS cluster and ECR repository are correctly configured and accessible.
5. Confirm that a Dockerfile exists in your repository. If using a custom path, ensure `DOCKERFILE_PATH` is set correctly.
6. Verify that the specified Kubernetes namespace exists in your cluster.
7. Ensure that the application port specified matches the port your application is listening on.

## Contributing

Contributions to improve the workflow are welcome. Please feel free to submit issues or pull requests.

## License

This workflow is available under the [MIT License](LICENSE).
