# Jenkins -- Pro

### 1. **Pipeline Fundamentals**
   - **Declarative vs Scripted Pipelines**: Understand the syntax and structure of both.
   - **Stages and Steps**: How to define different stages (build, test, deploy) in a pipeline.
   - **Post Actions**: Use of post actions like `success`, `failure`, and `always` in pipelines.
   - **Pipeline Triggers**: `cron`, `pollSCM`, `Git hooks` integration for scheduling builds.
   - **Pipeline as Code (Jenkinsfile)**: Creating and managing pipelines through `Jenkinsfile`.
   - **Multibranch Pipelines**: Automatically managing pipelines for multiple branches.

### 2. **Jenkins Agents and Nodes**
   - **Master-Agent Architecture**: Setting up master and agent nodes.
   - **Labeling Nodes**: Assigning specific jobs to specific agents using node labels.
   - **Connecting Nodes**: Using SSH or JNLP to connect agents.
   - **Distributed Builds**: Running builds on multiple nodes.

### 3. **Build Tools Integration**
   - **Maven/Gradle Integration**: Configuring Jenkins jobs to run Maven/Gradle builds.
   - **Docker Integration**: Running builds inside Docker containers.
   - **Ant Integration**: For Java builds using Ant.
   - **NPM/Yarn**: Integration with Node.js projects for frontend and backend.

### 4. **SCM Integration**
   - **GitHub, GitLab, Bitbucket Integration**: Configuring Jenkins to pull code from repositories.
   - **Git Branching and Pull Requests**: Handling feature branches, merging pull requests.
   - **Private Repositories**: Setting up credentials for private Git repositories.
   - **SCM Polling and Webhooks**: Automatically triggering builds based on SCM changes.

### 5. **Jenkins Plugins**
   - **Essential Plugins**: Git plugin, Pipeline plugin, Docker plugin, Maven plugin, etc.
   - **Credentials Plugin**: Managing credentials securely in Jenkins.
   - **Notification Plugins**: Slack, Email notifications.
   - **JUnit Plugin**: Parsing and displaying test results in Jenkins.

### 6. **Jenkins Pipeline Syntax**
   - **Environment Variables**: Setting and accessing environment variables in pipelines.
   - **Credentials Management**: Using credentials securely in pipelines (e.g., `withCredentials` block).
   - **Parallel Execution**: Running stages in parallel for faster builds.
   - **Shared Libraries**: Using shared pipeline libraries to centralize common pipeline code.
   - **Input/Approval**: Using `input` step for manual approval during the pipeline.

### 7. **Groovy Scripting**
   - **Why Groovy in Jenkins?**: Groovy is used for scripting within pipelines.
   - **Basic Groovy Syntax**: Loops, conditions, and function definitions.
   - **Writing Shared Libraries**: Creating reusable pipeline scripts using Groovy.
   - **Try-Catch Blocks**: Using Groovy for error handling in pipelines.

### 8. **Job Management**
   - **Freestyle Jobs**: Setting up and configuring basic Jenkins jobs.
   - **Parameterized Jobs**: Creating jobs with parameters (string, choice, boolean).
   - **Pipeline Jobs**: Configuring jobs that run Jenkinsfiles.
   - **Build Triggers**: Automatically triggering jobs (e.g., SCM changes, scheduled jobs).
   - **Build Artifacts**: Archiving build artifacts and accessing them post-build.

### 9. **Jenkins Security**
   - **User Roles and Permissions**: Role-based access control (RBAC).
   - **Securing Credentials**: How to store and manage secrets (e.g., API keys, passwords).
   - **SSH Key Setup**: Configuring Jenkins with SSH keys for Git-based operations.
   - **Jenkins Global Security**: Authentication, Authorization, and Security Matrix.

### 10. **CI/CD Concepts**
   - **Continuous Integration (CI)**: Automatically building and testing on code changes.
   - **Continuous Delivery (CD)**: Automatically deploying to staging/production after tests pass.
   - **Blue/Green Deployment**: Understanding deployment strategies and how Jenkins pipelines can facilitate these.
   - **Rolling Updates**: Deploying applications gradually, one part at a time.
   - **Docker-Based Pipelines**: Using Docker containers for CI/CD in Jenkins.

### 11. **Handling Notifications**
   - **Email Notifications**: Setting up email notifications for build status (success/failure).
   - **Slack Notifications**: Using the Slack plugin to send notifications to a Slack channel.
   - **Build Badges**: Showing the build status with badges (e.g., on GitHub).

### 12. **Pipeline Parallelization and Conditional Execution**
   - **Parallel Stages**: Running multiple stages simultaneously for faster execution.
   - **When Conditionals**: Adding conditionals for stage execution (e.g., only on certain branches).

### 13. **Artifact Management**
   - **Archiving Artifacts**: Storing and accessing build artifacts (e.g., `.war`, `.jar` files).
   - **Stashing and Unstashing**: Transferring files between stages.
   - **Publishing to Artifact Repositories**: Deploying build artifacts to Nexus or Artifactory.

### 14. **Jenkins as Code**
   - **Infrastructure as Code (IaC)**: Managing Jenkins configurations as code.
   - **Jenkins Configuration as Code (JCasC)**: Managing Jenkins system configurations as YAML code.

### 15. **Backup and Restore Jenkins**
   - **Backing Up Jenkins**: How to back up jobs, configurations, and plugins.
   - **Restoring Jenkins**: Restoring Jenkins from backup.

---

### Groovy in Jenkins:
- **Why Groovy?**: Groovy is used in Jenkins because it is a powerful scripting language that allows complex automation, especially for creating pipelines.
- **How to Use**: Groovy is used within the `Jenkinsfile` to write custom logic, control flows, and functions. It can handle error handling (`try-catch`), loops, and more.
- **Groovy in Declarative Pipelines**: Although declarative pipelines aim to limit scripting, certain logic (like looping, custom conditions) is still handled with Groovy.
- **Groovy in Scripted Pipelines**: In scripted pipelines, Groovy is fully integrated, allowing a higher degree of flexibility and customization.

---

### Ansible:
For your **Ansible practical viva**, expect topics like:
- **Inventory Management**: Handling static and dynamic inventories.
- **Ad-Hoc Commands**: Running commands directly against hosts (e.g., `ansible all -m ping`).
- **Playbooks**: Writing and managing Ansible playbooks.
- **Roles**: Organizing playbooks with roles.
- **Handlers**: Setting up tasks that only run when triggered by other tasks.
- **Templates**: Using Jinja2 templates for dynamic configuration.
- **Facts and Variables**: Gathering facts and using variables.
- **Conditionals and Loops**: Conditional task execution and loops in playbooks.
- **Vault**: Encrypting sensitive data with `ansible-vault`.
- **Modules**: Familiarity with essential Ansible modules like `yum`, `apt`, `copy`, `command`, `service`, `shell`, and `file`.
  
---

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

---

In Jenkins, the **Master-Agent architecture** allows you to distribute your build workload across multiple machines. This architecture consists of a master node that manages the Jenkins environment and one or more agent nodes (also known as slaves) that perform the actual build tasks. Here’s a detailed overview of Jenkins Agents and Nodes, including how to set up a master-agent architecture, label nodes, connect nodes, and manage distributed builds.

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

---

Integrating build tools with Jenkins allows you to automate the build and deployment process for various types of projects. Here’s an in-depth overview of how to integrate **Maven**, **Gradle**, **Docker**, **Ant**, and **NPM/Yarn** with Jenkins, along with examples for each.

---

## 1. Maven/Gradle Integration

### Maven Integration

Maven is a popular build automation tool used primarily for Java projects.

#### Steps to Configure Jenkins for Maven Builds

1. **Install the Maven Plugin**:
   - Go to Jenkins Dashboard > Manage Jenkins > Manage Plugins.
   - Under the **Available** tab, search for "Maven Integration" and install the plugin.

2. **Configure Maven in Jenkins**:
   - Go to Jenkins Dashboard > Manage Jenkins > Global Tool Configuration.
   - Under the **Maven** section, add a new Maven installation by specifying the name and selecting the version.

3. **Create a New Maven Job**:
   - Go to Jenkins Dashboard > New Item.
   - Enter a name for the job and select **Maven Project**.

4. **Configure the Job**:
   - In the job configuration, under **Source Code Management**, specify the repository URL.
   - Under **Build**, enter the goals (e.g., `clean install`).

#### Example Pipeline for Maven:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }
    }
}
```

### Gradle Integration

Gradle is another popular build tool that can be used for Java projects and more.

#### Steps to Configure Jenkins for Gradle Builds

1. **Install the Gradle Plugin**:
   - Go to Jenkins Dashboard > Manage Jenkins > Manage Plugins.
   - Under the **Available** tab, search for "Gradle" and install the plugin.

2. **Configure Gradle in Jenkins**:
   - Go to Jenkins Dashboard > Manage Jenkins > Global Tool Configuration.
   - Under the **Gradle** section, add a new Gradle installation.

3. **Create a New Gradle Job**:
   - Go to Jenkins Dashboard > New Item.
   - Enter a name for the job and select **Freestyle project** or **Pipeline**.

4. **Configure the Job**:
   - Under **Build**, select **Invoke Gradle Script** and enter the tasks (e.g., `build`).

#### Example Pipeline for Gradle:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'gradle build'
                }
            }
        }
    }
}
```

---

## 2. Docker Integration

Integrating Docker with Jenkins allows you to run builds inside Docker containers, ensuring consistency across environments.

### Steps to Configure Jenkins for Docker Builds

1. **Install the Docker Plugin**:
   - Go to Jenkins Dashboard > Manage Jenkins > Manage Plugins.
   - Under the **Available** tab, search for "Docker" and install the plugin.

2. **Configure Docker in Jenkins**:
   - Go to Jenkins Dashboard > Manage Jenkins > Global Tool Configuration.
   - Under the **Docker** section, specify the Docker installation details (if needed).

3. **Create a Docker Pipeline Job**:
   - Go to Jenkins Dashboard > New Item.
   - Enter a name for the job and select **Pipeline**.

4. **Configure the Job**:
   - Use Docker commands within the pipeline script to build, run, or interact with Docker images and containers.

#### Example Pipeline for Docker:

```groovy
pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t my-app .'
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                    sh 'docker run -d -p 8080:8080 my-app'
                }
            }
        }
    }
}
```

---

## 3. Ant Integration

Apache Ant is a Java library and command-line tool used for automating software build processes.

### Steps to Configure Jenkins for Ant Builds

1. **Install the Ant Plugin**:
   - Go to Jenkins Dashboard > Manage Jenkins > Manage Plugins.
   - Under the **Available** tab, search for "Ant" and install the plugin.

2. **Configure Ant in Jenkins**:
   - Go to Jenkins Dashboard > Manage Jenkins > Global Tool Configuration.
   - Under the **Ant** section, add a new Ant installation.

3. **Create a New Ant Job**:
   - Go to Jenkins Dashboard > New Item.
   - Enter a name for the job and select **Freestyle project**.

4. **Configure the Job**:
   - Under **Build**, select **Invoke Ant** and specify the targets (e.g., `compile`).

#### Example Pipeline for Ant:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'ant compile'
                }
            }
        }
    }
}
```

---

## 4. NPM/Yarn Integration

NPM (Node Package Manager) and Yarn are popular package managers for Node.js projects, enabling you to manage dependencies efficiently.

### Steps to Configure Jenkins for NPM/Yarn Builds

1. **Install NodeJS Plugin**:
   - Go to Jenkins Dashboard > Manage Jenkins > Manage Plugins.
   - Under the **Available** tab, search for "NodeJS" and install the plugin.

2. **Configure Node.js in Jenkins**:
   - Go to Jenkins Dashboard > Manage Jenkins > Global Tool Configuration.
   - Under the **NodeJS** section, add a new Node.js installation by specifying the name and version.

3. **Create a New NPM/Yarn Job**:
   - Go to Jenkins Dashboard > New Item.
   - Enter a name for the job and select **Pipeline**.

4. **Configure the Job**:
   - Use NPM or Yarn commands within the pipeline script to install dependencies and run scripts.

#### Example Pipeline for NPM:

```groovy
pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
    }
}
```

#### Example Pipeline for Yarn:

```groovy
pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'yarn install'
                }
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    sh 'yarn test'
                }
            }
        }
    }
}
```

---

## Conclusion

By integrating various build tools such as Maven, Gradle, Docker, Ant, and NPM/Yarn with Jenkins, you can automate and streamline your build and deployment processes. This integration enhances the development workflow, increases efficiency, and helps ensure consistent builds across different environments. Each tool has its specific configurations and benefits, so choose the ones that best fit your project needs.

---

Here’s a list of common build goals for **Maven**, **Gradle**, **Ant**, **NPM**, and **Yarn**, along with examples for each:

### Maven Goals with Examples

1. **clean**:
   - **Description**: Removes the `target` directory.
   - **Example**: 
     ```bash
     mvn clean
     ```

2. **install**:
   - **Description**: Compiles the code and installs the artifact into the local repository.
   - **Example**: 
     ```bash
     mvn install
     ```

3. **package**:
   - **Description**: Compiles the code and packages it into its distributable format (JAR/WAR).
   - **Example**: 
     ```bash
     mvn package
     ```

4. **test**:
   - **Description**: Runs the tests defined in the project.
   - **Example**: 
     ```bash
     mvn test
     ```

5. **validate**:
   - **Description**: Validates the project structure and configuration.
   - **Example**: 
     ```bash
     mvn validate
     ```

6. **deploy**:
   - **Description**: Copies the packaged artifact to the remote repository.
   - **Example**: 
     ```bash
     mvn deploy
     ```

7. **site**:
   - **Description**: Generates site documentation for the project.
   - **Example**: 
     ```bash
     mvn site
     ```

---

### Gradle Goals (Tasks) with Examples

1. **build**:
   - **Description**: Assembles and tests the project, creating the final artifacts.
   - **Example**: 
     ```bash
     gradle build
     ```

2. **clean**:
   - **Description**: Deletes the build directory (usually `build/`).
   - **Example**: 
     ```bash
     gradle clean
     ```

3. **assemble**:
   - **Description**: Creates the outputs without running tests.
   - **Example**: 
     ```bash
     gradle assemble
     ```

4. **check**:
   - **Description**: Runs all checks, including tests and style checks.
   - **Example**: 
     ```bash
     gradle check
     ```

5. **test**:
   - **Description**: Runs the unit tests for the project.
   - **Example**: 
     ```bash
     gradle test
     ```

6. **deploy**:
   - **Description**: Copies the artifacts to a repository for sharing.
   - **Example**: 
     ```bash
     gradle deploy
     ```

---

### Ant Targets with Examples

1. **clean**:
   - **Description**: Deletes previously compiled files and directories.
   - **Example**: 
     ```bash
     ant clean
     ```

2. **compile**:
   - **Description**: Compiles the source code into binary files.
   - **Example**: 
     ```bash
     ant compile
     ```

3. **jar**:
   - **Description**: Packages the compiled classes into a JAR file.
   - **Example**: 
     ```bash
     ant jar
     ```

4. **test**:
   - **Description**: Runs unit tests defined in the project.
   - **Example**: 
     ```bash
     ant test
     ```

5. **dist**:
   - **Description**: Creates a distribution of the application.
   - **Example**: 
     ```bash
     ant dist
     ```

---

### NPM Commands with Examples

1. **install**:
   - **Description**: Installs the dependencies listed in `package.json`.
   - **Example**: 
     ```bash
     npm install
     ```

2. **run <script>**:
   - **Description**: Executes a specified script defined in `package.json`.
   - **Example**: 
     ```bash
     npm run test
     ```

3. **test**:
   - **Description**: Runs the test script defined in `package.json`.
   - **Example**: 
     ```bash
     npm test
     ```

4. **build**:
   - **Description**: Compiles and prepares the application for production.
   - **Example**: 
     ```bash
     npm run build
     ```

5. **start**:
   - **Description**: Starts the application.
   - **Example**: 
     ```bash
     npm start
     ```

---

### Yarn Commands with Examples

1. **install**:
   - **Description**: Installs the dependencies listed in `package.json`.
   - **Example**: 
     ```bash
     yarn install
     ```

2. **run <script>**:
   - **Description**: Executes a specified script defined in `package.json`.
   - **Example**: 
     ```bash
     yarn run test
     ```

3. **test**:
   - **Description**: Runs the test script defined in `package.json`.
   - **Example**: 
     ```bash
     yarn test
     ```

4. **build**:
   - **Description**: Compiles and prepares the application for production.
   - **Example**: 
     ```bash
     yarn build
     ```

5. **start**:
   - **Description**: Starts the application.
   - **Example**: 
     ```bash
     yarn start
     ```
---
Here's an in-depth look at SCM (Source Control Management) integration in Jenkins, focusing on integrating with GitHub, GitLab, and Bitbucket, along with explanations for handling branching, private repositories, and build triggers.

### 1. SCM Integration

#### GitHub, GitLab, Bitbucket Integration
- **Description**: Jenkins can be configured to pull code from repositories hosted on platforms like GitHub, GitLab, and Bitbucket. This allows for seamless integration of code changes into the build and deployment process.

##### **Example Configuration:**
1. **Install Git Plugin**: Ensure that the Git plugin is installed in Jenkins (Manage Jenkins > Manage Plugins).
2. **Create a New Job**:
   - Go to **New Item** in Jenkins.
   - Choose **Freestyle project** or **Pipeline**, then click **OK**.
3. **Configure SCM**:
   - In the job configuration, scroll down to **Source Code Management**.
   - Select **Git** and enter the repository URL (e.g., `https://github.com/user/repo.git`).
   - Add any necessary credentials (see the **Private Repositories** section below for details).

---

### 2. Git Branching and Pull Requests

- **Description**: Jenkins can handle feature branches and pull requests by allowing builds for different branches and providing integration with pull request events.

##### **Example Handling:**
1. **Branch Specifier**:
   - In the job configuration under **Source Code Management**, you can specify the branch to build (e.g., `*/main` or `*/feature/*` for all feature branches).
   
2. **Pull Request Builds**:
   - For GitHub:
     - Use the **GitHub Branch Source Plugin**.
     - Configure a Multibranch Pipeline to automatically create jobs for each branch and PR.
     - This will allow Jenkins to build and test pull requests before merging.

3. **Webhook Configuration**:
   - In your GitHub repository, go to **Settings > Webhooks**.
   - Add a new webhook pointing to `http://<your-jenkins-url>/github-webhook/` to trigger builds on PR events.

---

### 3. Private Repositories

- **Description**: Accessing private repositories requires setting up credentials in Jenkins to authenticate the SCM requests.

##### **Example Configuration:**
1. **Add Credentials**:
   - Go to **Manage Jenkins > Manage Credentials**.
   - Select the appropriate domain (or global).
   - Click on **Add Credentials** and select **Username with password** or **Secret text** (for tokens).
   - Enter your GitHub/GitLab/Bitbucket username and password or access token.
   
2. **Link Credentials to Job**:
   - In the job configuration, under **Source Code Management**, select the credentials you added from the **Credentials** dropdown.

---

### 4. SCM Polling and Webhooks

- **Description**: Jenkins can automatically trigger builds based on changes in the SCM. This can be done either through polling the repository or using webhooks for instant triggers.

##### **SCM Polling Configuration**:
1. **Polling Configuration**:
   - In the job configuration, under **Build Triggers**, select **Poll SCM**.
   - Enter a cron-like schedule to define how often Jenkins checks for changes (e.g., `H/5 * * * *` to poll every 5 minutes).

##### **Webhook Configuration**:
- **For GitHub**:
   - In your repository settings, under **Webhooks**, add a new webhook with the following:
     - **Payload URL**: `http://<your-jenkins-url>/github-webhook/`
     - **Content Type**: `application/json`
     - **Events**: Choose **Just the push event** or other events based on your needs.
     
- **For GitLab**:
   - In your project settings, go to **Webhooks** and configure a similar webhook.
   - Use the same Payload URL format.

---

### Summary

Integrating Jenkins with GitHub, GitLab, and Bitbucket allows for streamlined continuous integration and deployment processes. Setting up SCM integration involves configuring repository URLs, handling credentials for private repositories, and automating build triggers using polling or webhooks. By managing branches and pull requests effectively, Jenkins can ensure that the code is always tested and ready for deployment.

---

Here's an in-depth overview of Jenkins Pipeline syntax, covering essential concepts like environment variables, credentials management, parallel execution, shared libraries, and input/approval processes.

### Jenkins Pipeline Syntax Overview

#### 1. Environment Variables
- **Setting and Accessing Environment Variables**: Environment variables can be defined in a pipeline to store configuration values that can be reused throughout the stages.

**Example**:
```groovy
pipeline {
    agent any
    environment {
        MY_ENV_VAR = 'Hello, World!'
    }
    stages {
        stage('Example') {
            steps {
                script {
                    echo "The value of MY_ENV_VAR is: ${MY_ENV_VAR}"
                }
            }
        }
    }
}
```
- **Explanation**: The `environment` block sets `MY_ENV_VAR`, which is accessed in the `script` block.

---

#### 2. Credentials Management
- **Using Credentials Securely**: Jenkins allows you to manage sensitive information securely using the `withCredentials` block.

**Example**:
```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'my-creds-id', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'git clone https://$USER:$PASS@github.com/my/repo.git'
                }
            }
        }
    }
}
```
- **Explanation**: The `withCredentials` block securely injects the username and password from Jenkins credentials, allowing them to be used in shell commands.

---

#### 3. Parallel Execution
- **Running Stages in Parallel**: You can define stages to run concurrently, which speeds up the overall pipeline execution.

**Example**:
```groovy
pipeline {
    agent any
    stages {
        stage('Parallel Stages') {
            parallel {
                stage('Stage 1') {
                    steps {
                        echo 'Running Stage 1'
                    }
                }
                stage('Stage 2') {
                    steps {
                        echo 'Running Stage 2'
                    }
                }
            }
        }
    }
}
```
- **Explanation**: Both "Stage 1" and "Stage 2" execute simultaneously, reducing total build time.

---

#### 4. Shared Libraries
- **Using Shared Pipeline Libraries**: Shared libraries allow you to centralize common code used across multiple pipelines, improving maintainability and reusability.

**Example**:
1. Create a shared library repository with a `vars` directory containing a Groovy script (e.g., `myUtils.groovy`):
   ```groovy
   def greet(String name) {
       return "Hello, ${name}!"
   }
   ```

2. Reference the shared library in your pipeline:
```groovy
@Library('my-shared-library') _
pipeline {
    agent any
    stages {
        stage('Greet') {
            steps {
                script {
                    def message = myUtils.greet('World')
                    echo message
                }
            }
        }
    }
}
```
- **Explanation**: The `@Library` annotation imports the shared library, allowing the use of its methods in the pipeline.

---

#### 5. Input/Approval
- **Using Input Step for Manual Approval**: The `input` step can pause the pipeline and wait for user approval before continuing.

**Example**:
```groovy
pipeline {
    agent any
    stages {
        stage('Approval Stage') {
            steps {
                script {
                    def userInput = input(
                        message: 'Approve to proceed?',
                        parameters: [string(name: 'Approval', defaultValue: 'yes', description: 'Type "yes" to approve')]
                    )
                    echo "User approved: ${userInput}"
                }
            }
        }
    }
}
```
- **Explanation**: The pipeline pauses at the input stage, waiting for the user to type "yes" to continue execution.

---

### Summary

Understanding Jenkins Pipeline syntax is crucial for creating robust and flexible CI/CD processes. Key concepts include:

- **Environment Variables**: Simplify configuration management across stages.
- **Credentials Management**: Safeguard sensitive information with `withCredentials`.
- **Parallel Execution**: Enhance build speed by running stages concurrently.
- **Shared Libraries**: Promote code reusability and maintainability across multiple pipelines.
- **Input/Approval**: Implement manual approval steps to ensure quality control in the pipeline flow.

By leveraging these features, you can create efficient and secure pipelines tailored to your development and deployment needs.
