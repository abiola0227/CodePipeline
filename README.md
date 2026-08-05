# CodePipeline

A CloudFormation-defined CI/CD pipeline for a Python application, provisioning CodeBuild, CodeDeploy, and CodePipeline, with automated deployment to EC2.

## Overview

This project defines a complete CI/CD pipeline as code using AWS CloudFormation templates — rather than manually clicking through the CodePipeline, CodeBuild, and CodeDeploy consoles, each stage of the pipeline is provisioned from a template. The pipeline builds, tests, and deploys a Python application to EC2 using CodeDeploy's lifecycle hook model.

## Pipeline Stages

1. **Build** — `buildspec.yml` defines the CodeBuild phases, installing dependencies from `requirements.txt` and running the application build.
2. **Test** — `test_app.py` runs automated tests against the application as part of the build process.
3. **Deploy** — `appspec.yml` defines the CodeDeploy deployment specification, orchestrating the deployment lifecycle through hook scripts:
   - `pre-install-checks.sh` — validates the target environment before deployment
   - `create-codedeploy-env-vars.sh` — sets up environment variables needed for the deployment
   - `stop.sh` — stops the currently running application
   - `run.sh` — starts the newly deployed application
   - `validate.sh` — confirms the deployment succeeded and the application is healthy

## Infrastructure as Code

Unlike a pipeline built manually through the AWS console, this repo provisions the pipeline itself from CloudFormation templates:

| Template | Provisions |
|---|---|
| `codebuild-template.yml` | The CodeBuild project |
| `codedeploy-template.yml` | The CodeDeploy application and deployment group |
| `codepipeline-template.yml` | The CodePipeline orchestrating source → build → deploy |

## Repo Contents

| File | Purpose |
|---|---|
| `app.py` | Python application source |
| `test_app.py` | Automated tests |
| `requirements.txt` | Python dependencies |
| `buildspec.yml` | AWS CodeBuild pipeline definition |
| `appspec.yml` | AWS CodeDeploy deployment specification |
| `codebuild-template.yml`, `codedeploy-template.yml`, `codepipeline-template.yml` | CloudFormation templates provisioning the pipeline infrastructure |
| `pre-install-checks.sh`, `create-codedeploy-env-vars.sh`, `run.sh`, `stop.sh`, `validate.sh` | CodeDeploy lifecycle hook scripts |

## Tech Stack

Python · AWS CodePipeline · AWS CodeBuild · AWS CodeDeploy · AWS CloudFormation · EC2
