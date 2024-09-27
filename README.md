# Jenkins -- Pro

## 1. Declarative vs. Scripted Pipelines

### Declarative Pipelines

**Declarative pipelines** are designed to provide a simpler and more readable syntax. They use a defined structure and syntax, making it easier for users to understand the pipeline's intent.

**Example:**

```groovy
pipeline {
    agent any  // Defines the agent (executor) for the pipeline

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // Add your build commands here
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                // Add your test commands here
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deployment commands here
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

### Scripted Pipelines

**Scripted pipelines** offer more flexibility and power by allowing users to write complex logic in the pipeline using Groovy syntax. They are more suitable for advanced users who require full control.

**Example:**

```groovy
node {
    try {
        stage('Build') {
            echo 'Building...'
            // Add your build commands here
        }
        stage('Test') {
            echo 'Testing...'
            // Add your test commands here
        }
        stage('Deploy') {
            echo 'Deploying...'
            // Add your deployment commands here
        }
    } catch (Exception e) {
        echo "Pipeline failed: ${e.message}"
    } finally {
        // Post actions
        echo 'Cleanup actions can be performed here.'
    }
}
```

---

## 2. Stages and Steps

**Stages** represent a phase in the pipeline, while **steps** are the actual commands or actions that are executed within a stage.

### Example:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Compiling code...'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
}
```

---

## 3. Post Actions

**Post actions** are executed after the stages have been run, and they can be configured to execute based on the result of the pipeline (success, failure, or always).

### Example:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }

    post {
        success {
            echo 'Build was successful!'
        }
        failure {
            echo 'Build failed.'
        }
        always {
            echo 'This will always run, regardless of the build result.'
        }
    }
}
```

---

## 4. Pipeline Triggers

Triggers allow the pipeline to start automatically based on certain conditions like time (cron), source code changes (pollSCM), or external events (Git hooks).

### Example using Cron:

```groovy
pipeline {
    agent any
    triggers {
        cron('H 2 * * 1') // Triggers the pipeline every Monday at 2 AM
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }
}
```

### Example using pollSCM:

```groovy
pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *') // Polls the SCM every 5 minutes
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
    }
}
```

---

## 5. Pipeline as Code (Jenkinsfile)

**Pipeline as Code** allows you to define your build process in a Jenkinsfile, making it easy to version control and manage. The Jenkinsfile can be placed in the root of your repository.

### Example Jenkinsfile:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to server...'
                sh 'scp target/myapp.war user@server:/path/to/deploy'
            }
        }
    }
}
```

---

## 6. Multibranch Pipelines

**Multibranch Pipelines** allow Jenkins to automatically discover and manage branches in a source code repository. This is particularly useful for projects with multiple branches, enabling each branch to have its own pipeline.

### Example:

1. **Create a Multibranch Pipeline in Jenkins:**
   - Go to Jenkins Dashboard > New Item.
   - Choose "Multibranch Pipeline."
   - Configure the Git repository where your branches are located.

2. **Jenkinsfile for Each Branch:**
   Each branch can have its own `Jenkinsfile` with specific instructions.

**Example Jenkinsfile for the `develop` branch:**

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building from develop branch...'
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests from develop branch...'
                sh 'mvn test'
            }
        }
    }
}
```

**Example Jenkinsfile for the `feature` branch:**

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building from feature branch...'
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests from feature branch...'
                sh 'mvn test'
            }
        }
    }
}
```

---

## Conclusion

By understanding these fundamental concepts of Jenkins Pipelines, you can effectively automate your CI/CD processes and manage your application development lifecycle. Whether using Declarative or Scripted syntax, leveraging stages, post actions, triggers, and multibranch management will empower you to build robust, automated workflows tailored to your needs.

