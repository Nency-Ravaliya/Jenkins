In Jenkins pipelines, **Scripted pipelines** support functions natively, while **Declarative pipelines** have limited function support but can use functions inside `script` blocks.

### 1. **Scripted Pipeline (Supports Functions Natively)**

In a **scripted pipeline**, you can define and call functions like you would in standard Groovy code. Scripted pipelines give you full control over Groovy's features, including functions.

#### Example:
```groovy
def myFunction() {
    echo "This is a function in a scripted pipeline!"
}

node {
    stage('Run Function') {
        myFunction()
    }
}
```

In this example, the `myFunction()` is defined and called within the pipeline without any special handling.

### 2. **Declarative Pipeline (Limited Function Support)**

In a **declarative pipeline**, you can't directly define functions at the top level. However, you can use functions inside the `script` block, which allows you to run Groovy code with full functionality.

#### Example:
```groovy
pipeline {
    agent any

    stages {
        stage('Run Function') {
            steps {
                script {
                    def myFunction() {
                        echo "This is a function in a declarative pipeline!"
                    }
                    myFunction()
                }
            }
        }
    }
}
```

Here, the function `myFunction()` is defined inside a `script` block, which allows for Groovy's full syntax within a declarative pipeline.

### Summary:
- **Scripted Pipeline**: Functions are natively supported and can be defined at the top level.
- **Declarative Pipeline**: Functions need to be placed inside a `script` block, which is used to run Groovy code within a declarative pipeline.

If you're using functions frequently or need complex logic, **scripted pipelines** offer more flexibility. However, **declarative pipelines** can still use functions with the help of `script` blocks for specific use cases.
