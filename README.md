
# Hosting a Site on AWS EC2 with CI/CD Setup Using GitHub Actions

## Overview

This repository demonstrates how to host a static website on AWS EC2 using a Continuous Integration and Continuous Deployment (CI/CD) setup with GitHub Actions. The goal is to automate the deployment process, making it seamless to push changes from your local development environment to a live server.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [GitHub Actions Workflow](#github-actions-workflow)
- [Usage](#usage)
- [License](#license)

## Prerequisites

Before you begin, ensure you have the following:

- An AWS account.
- An EC2 instance set up to host your website.
- A domain name (optional) pointing to your EC2 instance.
- Git installed on your local machine.
- A basic understanding of GitHub Actions and CI/CD concepts.

## Setup

1. **Create an EC2 Instance**:
   - Launch an EC2 instance (Amazon Linux or any preferred AMI) in your AWS account.
   - Ensure that the security group associated with your instance allows inbound SSH (port 22) and HTTP (port 80) traffic.

2. **Configure SSH Access**:
   - Create a key pair for SSH access and download the private key (`.pem` file).
   - Ensure the public key is added to the EC2 instance.

3. **Clone This Repository**:
   - Run the following commands in your terminal:
     ```
     git clone https://github.com/yourusername/your-repo-name.git
     cd your-repo-name
     ```

Continue with the rest of your README as needed.


4. **Set Up GitHub Secrets**:
   In your GitHub repository, go to **Settings > Secrets and Variables > Actions** and add the following secrets:
   - `EC2_SSH_KEY`: Your private SSH key content.
   - `HOST_DNS`: Public DNS name or IP address of your EC2 instance.
   - `USERNAME`: The SSH username for your EC2 instance (e.g., `ec2-user`).
   - `TARGET_DIR`: The target directory on your EC2 instance where the website files will be deployed (e.g., `/var/www/html`).

## GitHub Actions Workflow

The GitHub Actions workflow is defined in `.github/workflows/deploy.yml`. It automates the deployment process by:

- Triggering on push events to the main branch.
- Deploying the latest changes to the specified EC2 instance using `rsync`.

### Example Workflow File

```yaml
name: Push-to-EC2

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the files
        uses: actions/checkout@v2

      - name: Deploy to EC2
        uses: easingthemes/ssh-deploy@main
        env:
          SSH_PRIVATE_KEY: ${{ secrets.EC2_SSH_KEY }}
          REMOTE_HOST: ${{ secrets.HOST_DNS }}
          REMOTE_USER: ${{ secrets.USERNAME }}
          TARGET: ${{ secrets.TARGET_DIR }}
```

## Usage

1. **Make Changes**: Update your website files locally.
2. **Commit and Push**:
   ```bash
   git add .
   git commit -m "Update website files"
   git push origin main
   ```

3. **Deployment**: Upon pushing to the main branch, the GitHub Actions workflow will automatically trigger and deploy your changes to the EC2 instance.

## License

This project is licensed under the MIT License. See the LICENSE file for more details.

```

### Instructions
- Replace `yourusername` and `your-repo-name` with your actual GitHub username and repository name.
- Adjust any specific details to fit your project and requirements.

Feel free to copy and paste this code into your `README.md` file! Let me know if you need any more assistance!
