Here is a **complete declarative pipeline** example with parameters and environment variables. This example demonstrates how to use string and boolean parameters, environment variables, and conditional logic within a Jenkins pipeline.

### Declarative Pipeline with Parameters and Environment Variables

```groovy
pipeline {
    agent any
    
    // Define the parameters block to accept user inputs
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
        booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Deploy after build?')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'production'], description: 'Select the environment')
    }
    
    // Define environment variables used in the pipeline
    environment {
        APP_NAME = 'MyApp' // Static environment variable
        BUILD_DIR = 'build' // Another static environment variable
        TARGET_ENV = "${params.ENVIRONMENT}" // Dynamic, based on parameter
    }

    stages {
        // Checkout code stage
        stage('Checkout') {
            steps {
                echo "Checking out branch: ${params.BRANCH_NAME}"
                // Git checkout using the parameter value
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/your-repo.git'
            }
        }
        
        // Build stage
        stage('Build') {
            steps {
                echo "Building ${APP_NAME}..."
                sh '''
                    mkdir -p ${BUILD_DIR}
                    echo "Building application in ${BUILD_DIR} directory..."
                    # Simulate a build process
                    touch ${BUILD_DIR}/output.txt
                '''
            }
        }
        
        // Testing stage (could be unit tests, integration tests, etc.)
        stage('Test') {
            steps {
                echo "Running tests for ${APP_NAME} in environment: ${TARGET_ENV}"
                // Simulate running tests
                sh '''
                    echo "Running tests for ${APP_NAME} in ${TARGET_ENV} environment..."
                    # Test logic goes here
                '''
            }
        }

        // Deploy stage, only runs if DEPLOY parameter is true
        stage('Deploy') {
            when {
                expression {
                    return params.DEPLOY // Only execute if DEPLOY is set to true
                }
            }
            steps {
                echo "Deploying ${APP_NAME} to ${TARGET_ENV} environment"
                // Simulate deployment process
                sh '''
                    echo "Deploying ${APP_NAME} to ${TARGET_ENV} environment..."
                    # Deployment logic goes here
                '''
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace..."
            deleteDir() // Clean up the workspace after the job
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
```

### Breakdown of the Pipeline:

1. **Parameters**:
    - `BRANCH_NAME`: Defines which Git branch to build.
    - `DEPLOY`: Boolean parameter to decide whether to run the deploy stage.
    - `ENVIRONMENT`: Choice parameter to select the environment (dev, staging, production).

2. **Environment Variables**:
    - `APP_NAME`: A static environment variable for the application name.
    - `BUILD_DIR`: A static environment variable for the build directory.
    - `TARGET_ENV`: A dynamic environment variable that captures the selected environment from the parameters.

3. **Stages**:
    - **Checkout**: Pulls the code from the selected Git branch.
    - **Build**: Simulates a build process by creating a build directory and output file.
    - **Test**: Simulates running tests for the application in the selected environment.
    - **Deploy**: This stage only runs if the `DEPLOY` parameter is set to true. It simulates deploying the application to the selected environment.

4. **Post Actions**:
    - **always**: Cleans up the workspace after the pipeline finishes.
    - **success**: Prints a success message if the pipeline completes successfully.
    - **failure**: Prints a failure message if the pipeline fails.

### Running the Pipeline:

- When you trigger this pipeline, Jenkins will prompt you to provide values for the parameters.
- Based on your input, it will check out the specified branch, build the application, run tests, and optionally deploy the application depending on the `DEPLOY` parameter.
