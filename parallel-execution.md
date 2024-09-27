Here are examples of using parallel execution in both **declarative** and **scripted** pipelines in Jenkins.

### 1. **Declarative Pipeline: Parallel Execution**

In a declarative pipeline, parallel execution is straightforward using the `parallel` block. Here's an example where two tasks (Task A and Task B) run simultaneously.

```groovy
pipeline {
    agent any
    stages {
        stage('Parallel Execution') {
            parallel {
                stage('Task A') {
                    steps {
                        echo "Running Task A"
                        sh 'sleep 5' // Simulating a long-running task
                        echo "Task A Completed"
                    }
                }
                stage('Task B') {
                    steps {
                        echo "Running Task B"
                        sh 'sleep 3' // Simulating a long-running task
                        echo "Task B Completed"
                    }
                }
            }
        }
    }
}
```

In this pipeline, both **Task A** and **Task B** will run in parallel, reducing the overall time of the pipeline execution.

### 2. **Scripted Pipeline: Parallel Execution**

In a scripted pipeline, you can achieve parallel execution using the `parallel` function with a map to define the different tasks. Here's an example:

```groovy
node {
    stage('Parallel Execution') {
        parallel(
            'Task A': {
                echo "Running Task A"
                sh 'sleep 5' // Simulating a long-running task
                echo "Task A Completed"
            },
            'Task B': {
                echo "Running Task B"
                sh 'sleep 3' // Simulating a long-running task
                echo "Task B Completed"
            }
        )
    }
}
```

In the scripted pipeline, the `parallel` function takes a map where the keys are the names of the tasks and the values are the closures that define the actions for each task. This will also run **Task A** and **Task B** concurrently.

### Key Differences:
- **Declarative**: Uses the `parallel` block inside `stages` and is easier to set up.
- **Scripted**: Uses the `parallel` method with closures for flexibility.

Both methods allow running multiple tasks simultaneously, saving time and optimizing resources, especially for tasks like testing across environments or platforms.
