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


In Jenkins, the **Master-Agent architecture** allows you to distribute your build workload across multiple machines. This architecture consists of a master node that manages the Jenkins environment and one or more agent nodes (also known as slaves) that perform the actual build tasks. Here’s a detailed overview of Jenkins Agents and Nodes, including how to set up a master-agent architecture, label nodes, connect nodes, and manage distributed builds.

---

## 1. Master-Agent Architecture

### Overview

- **Master Node**: The Jenkins master is responsible for managing the overall Jenkins environment. It schedules jobs, manages the user interface, and maintains the configuration.
- **Agent Nodes**: Agent nodes are separate machines that execute the build tasks. This allows you to distribute workloads and utilize resources more effectively.

### Setting Up Master and Agent Nodes

#### Step 1: Install Jenkins on the Master Node

1. **Install Jenkins**: Follow the installation instructions for your operating system (e.g., using `apt` for Ubuntu or `yum` for CentOS).
   
   ```bash
   # For Ubuntu
   sudo apt update
   sudo apt install jenkins
   ```

2. **Start Jenkins**: Ensure Jenkins is running.

   ```bash
   sudo systemctl start jenkins
   ```

3. **Access Jenkins**: Open a web browser and go to `http://your-master-ip:8080` to access the Jenkins dashboard.

#### Step 2: Set Up Agent Nodes

1. **Install Java**: Ensure Java is installed on the agent nodes as Jenkins requires Java to run.

   ```bash
   # Install Java
   sudo apt update
   sudo apt install openjdk-11-jdk
   ```

2. **Connect Agent Nodes to the Master Node**:
   - **Via SSH**: You can set up an SSH connection from the master to the agent.
   - **Via JNLP**: Alternatively, you can use JNLP for connecting the agent node to the master.

---

## 2. Labeling Nodes

**Node labeling** allows you to assign specific jobs to specific agents based on labels. This is useful when you have different types of jobs that require specific environments or resources.

### How to Label Nodes

1. **Access Node Configuration**:
   - Go to Jenkins dashboard > Manage Jenkins > Manage Nodes and Clouds.
   - Click on the agent node you want to label.

2. **Assign a Label**:
   - In the node configuration, find the **Labels** field and assign a label (e.g., `linux`, `windows`, `test`).
   - Save the configuration.

### Using Labels in Pipelines

You can specify the label in your pipeline script to ensure that the job runs on the correct agent.

#### Example:

```groovy
pipeline {
    agent { label 'linux' } // Only runs on nodes with the 'linux' label

    stages {
        stage('Build') {
            steps {
                echo 'Building on a Linux agent...'
            }
        }
    }
}
```

---

## 3. Connecting Nodes

Jenkins agents can be connected to the master node using two main methods: **SSH** and **JNLP**.

### SSH Connection

1. **Prerequisites**:
   - Ensure that SSH is enabled on the agent node.
   - The master node must have SSH access to the agent node.

2. **Configure Agent Node**:
   - In Jenkins, go to Manage Jenkins > Manage Nodes and Clouds > New Node.
   - Choose **Permanent Agent** and configure the details.
   - In the **Launch method** dropdown, select **Launch agents via SSH**.

3. **Provide Connection Details**:
   - Enter the hostname, credentials (SSH user), and any other required details.

### JNLP Connection

1. **Configure Agent Node**:
   - In Jenkins, go to Manage Jenkins > Manage Nodes and Clouds > New Node.
   - Choose **Permanent Agent** and configure the details.
   - In the **Launch method** dropdown, select **Launch agents via Java Web Start**.

2. **Download the JNLP File**:
   - Once the agent node is configured, you can download the JNLP file from the node's configuration page.

3. **Run the Agent on the Node**:
   - Use the following command to run the agent:
   ```bash
   java -jar agent.jar -jnlpUrl http://your-master-ip:8080/computer/your-agent-name/slave-agent.jnlp -secret your-agent-secret
   ```

---

## 4. Distributed Builds

**Distributed builds** allow Jenkins to execute jobs across multiple agent nodes, optimizing the use of resources and reducing build times.

### Benefits of Distributed Builds

- **Increased Efficiency**: Multiple builds can be processed simultaneously.
- **Resource Optimization**: Utilize various hardware and software environments for testing.
- **Scalability**: Easily add more agents as project demands increase.

### Example of Distributed Builds

You can configure jobs to run on specific agents or distribute them evenly among available agents.

#### Example Pipeline with Distributed Builds:

```groovy
pipeline {
    agent none // No default agent

    stages {
        stage('Build on Linux') {
            agent { label 'linux' }
            steps {
                echo 'Building on Linux agent...'
            }
        }
        stage('Build on Windows') {
            agent { label 'windows' }
            steps {
                echo 'Building on Windows agent...'
            }
        }
    }
}
```

In this example, two separate builds will be executed concurrently, one on a Linux agent and another on a Windows agent, leveraging the distributed build capabilities of Jenkins.

---

## Conclusion

By understanding Jenkins agents and nodes, you can effectively set up a master-agent architecture that optimizes your CI/CD process. Labeling nodes helps direct jobs to the appropriate environments, while distributed builds enhance efficiency and scalability, making Jenkins a powerful tool for managing complex software projects.

