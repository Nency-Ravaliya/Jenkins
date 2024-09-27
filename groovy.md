### Groovy Scripting in Jenkins

Groovy is the language Jenkins uses for its pipeline DSL (Domain-Specific Language). Groovy, a Java-syntax-compatible scripting language, is powerful for writing flexible, maintainable, and reusable pipelines.

### 1. Why Groovy in Jenkins?

- **Flexibility**: Groovy provides full scripting capabilities within Jenkins pipelines, allowing complex automation logic.
- **Integration with Java**: Since Groovy is Java-compatible, it can use all Java libraries, making it easy to extend Jenkins with custom logic.
- **Dynamic**: Groovy is dynamically typed, allowing easy modification and fast iteration.
- **Readable and Concise**: Groovy scripts are generally shorter and more readable than Java code.

---

### 2. Basic Groovy Syntax

#### a. **Variables and Types**
Groovy has dynamic typing but can also use static types.

**Example**:
```groovy
def name = "Jenkins"
int age = 5
println "Name: ${name}, Age: ${age}"
```

#### b. **Loops**

- **For Loop**:
```groovy
for (int i = 0; i < 5; i++) {
    println "Iteration ${i}"
}
```

- **Each Loop** (using Groovy's collection support):
```groovy
def names = ['Alice', 'Bob', 'Charlie']
names.each { name ->
    println "Hello, ${name}!"
}
```

#### c. **Conditions**

- **If-Else**:
```groovy
def x = 10
if (x > 5) {
    println 'x is greater than 5'
} else {
    println 'x is less than or equal to 5'
}
```

- **Switch Statement**:
```groovy
def fruit = 'apple'
switch (fruit) {
    case 'apple':
        println 'Fruit is apple'
        break
    case 'banana':
        println 'Fruit is banana'
        break
    default:
        println 'Unknown fruit'
}
```

#### d. **Function Definitions**

Functions in Groovy are defined using the `def` keyword. You can pass parameters and return values.

**Example**:
```groovy
def greet(String name) {
    return "Hello, ${name}!"
}

println greet('Jenkins')
```

---

### 3. Writing Shared Libraries in Groovy

**Shared Libraries** in Jenkins allow teams to create reusable code across pipelines. These libraries are written in Groovy.

#### Example of a Shared Library Function:
1. **Define a Function** (`vars/myUtils.groovy` in the shared library repository):
```groovy
def greet(String name) {
    return "Hello, ${name}!"
}
```

2. **Use it in a Pipeline**:
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
In this example, the shared library's `greet` function is invoked within the pipeline.

---

### 4. Try-Catch Blocks: Error Handling in Groovy

Error handling is important in Jenkins pipelines to ensure they can gracefully handle failures. Groovy's `try-catch-finally` allows error management.

**Example**:
```groovy
pipeline {
    agent any
    stages {
        stage('Error Handling Example') {
            steps {
                script {
                    try {
                        // Simulate an error
                        sh 'exit 1'
                    } catch (Exception e) {
                        // Handle error
                        echo "Caught an exception: ${e.getMessage()}"
                    } finally {
                        echo 'This block always runs, even if there is an error'
                    }
                }
            }
        }
    }
}
```

- **Try Block**: The code inside the `try` block is executed. If any error occurs, control is passed to the `catch` block.
- **Catch Block**: The error message is caught and handled.
- **Finally Block**: This block runs regardless of whether an error occurs.

---

### Example of a Complex Groovy-Based Jenkins Pipeline

```groovy
pipeline {
    agent any

    environment {
        GREETING = 'Hello'
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    println "${GREETING}, Jenkins!"
                }
            }
        }

        stage('Loop Example') {
            steps {
                script {
                    for (int i = 1; i <= 3; i++) {
                        echo "Iteration ${i}"
                    }
                }
            }
        }

        stage('Error Handling') {
            steps {
                script {
                    try {
                        // Fails on purpose
                        sh 'exit 1'
                    } catch (Exception e) {
                        echo "Error occurred: ${e.getMessage()}"
                    }
                }
            }
        }

        stage('Parallel Execution') {
            parallel {
                stage('Task 1') {
                    steps {
                        echo 'Running Task 1'
                    }
                }
                stage('Task 2') {
                    steps {
                        echo 'Running Task 2'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
```

### Key Concepts Demonstrated:

- **Environment Variables**: Defined using the `environment` block.
- **Loops**: Used in the "Loop Example" stage.
- **Try-Catch**: Demonstrated in the "Error Handling" stage for error management.
- **Parallel Execution**: Shown in the "Parallel Execution" stage.
- **Post Actions**: Ensures certain actions are taken depending on pipeline outcomes.

---

### Summary

- **Groovy in Jenkins**: It's the core language for writing flexible and dynamic Jenkins pipelines.
- **Basic Groovy Syntax**: Includes variables, loops, conditions, and functions.
- **Shared Libraries**: Let you write reusable functions for pipelines.
- **Error Handling**: Managed via `try-catch` blocks, ensuring robustness in pipelines.

Groovy’s flexibility and powerful scripting features make it a vital tool for managing complex Jenkins pipelines.
