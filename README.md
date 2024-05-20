# GitHub Actions Deploy Demo

This repository demonstrates a GitHub Actions workflow that builds a java gradle project and deploys the package using the [github-actions-deploy](https://github.com/digital-ai/github-actions-deploy) custom action. This guide will walk you through the setup and functionality of the workflow.

## Project Overview

This project is a Java Gradle project designed to demonstrate the use of GitHub Actions. The workflow defined in `build-deploy.yml` performs the following steps:
1. Checks out the code from the repository.
2. Sets up JDK 17.
3. Grants execute permissions to the Gradle wrapper script.
4. Builds the project using Gradle.
5. Creates, publishes, and deploys the package using `digital-ai/github-actions-deploy`.

## Workflow File

The main workflow file, `.github/workflows/build-deploy.yml`, is configured as follows:

```yaml
name: Build and Deploy Package

on: [workflow_dispatch]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK 17
        uses: actions/setup-java@v2
        with:
          distribution: 'adopt'
          java-version: '17'

      - name: Grant execute permissions to gradlew
        run: chmod +x ./gradlew

      - name: Build with Gradle
        id: build
        run: ./gradlew build --no-daemon --console=plain

      - name: Create Publish and Deploy Package
        id: deploy
        uses: digital-ai/github-actions-deploy@main
        with:
          serverUrl: ${{ secrets.SERVERURL }}
          username: ${{ secrets.USERNAME }}
          password: ${{ secrets.PASSWORD }}
          manifestPath: '/deployit-manifest.xml'
          action: 'create_publish_deploy'
          outputPath: '/outputdar'
          versionNumber: ${{ vars.VERSIONNUMBER }}
          packageName: 'appForAction-1.0.dar'
          environmentId: 'Environments/envForAction'
          rollback: 'yes'
```
## Steps Explained

1. **Checkout code**: Uses the `actions/checkout@v2` action to clone the repository.
2. **Set up JDK 17**: Uses the `actions/setup-java@v2` action to set up AdoptOpenJDK 17.
3. **Grant execute permissions to gradlew**: Runs a shell command to make the Gradle wrapper executable.
4. **Build with Gradle**: Executes the Gradle build command.
5. **Create Publish and Deploy Package**: Uses the `digital-ai/github-actions-deploy@main` action to create, publish, and deploy the package.

## Secrets and Variables

The workflow uses the following secrets and variables:
- `SERVERURL`: The URL of the deployment server.
- `USERNAME`: The username for the deployment server.
- `PASSWORD`: The password for the deployment server.
- `VERSIONNUMBER`: The version number for the package.

These secrets and variables need to be set in the repository settings under **Settings > Secrets and variables**.

## Running the Workflow

This workflow is triggered manually via the GitHub Actions interface using the `workflow_dispatch` event. To run the workflow:
1. Navigate to the **Actions** tab in your GitHub repository.
2. Select the **Build and Deploy Package** workflow.
3. Click the **Run workflow** button.

## Conclusion

This demo project showcases how to integrate `github-actions-deploy` for building and deploying a Java Gradle project. 