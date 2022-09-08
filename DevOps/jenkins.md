# BASIC SYNTAX

## What is a Jenkins Pipeline

A `pipeline` is a collection of jobs that brings software from vesrion contril into end-users by automation. Used in **Continuous Delivery** in software development workflow.

Uses `Groovy DSL` to write scripts for automation.

## Pipeline example

*pipeline*: must be top level , base structure of script
*agent*: where to execute, (available jenkins agent)*stages*: where work happens / different stages of pipeline

```Groovy
pipeline {
  agent any
  stages {
      stage('One') {
      steps {
          echo 'Hi, this is Dennis Joseph'
      }
      }
      stage('Two') {
      steps {
        input('Do you want to proceed?')
      }
      }
      stage('Three') {
      when {
            not {
                branch "master"
            }
      }
      steps {
            echo "Hello"
      }
      }
  }
}
```

2 Different types of Jenkins Pipeline:

   1. Declarative pipeline syntax
   2. Scripted pipeline syntax

### Declarative pipeline

In Declarative Pipeline syntax, the `pipeline` block defines all the work done throughout your entire Pipeline

```Jenkins
pipeline { 
    agent any 
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') { 
            steps { 
                sh 'make' 
            }
        }
        stage('Test'){
            steps {
                sh 'make check'
                junit 'reports/**/*.xml' 
            }
        }
        stage('Deploy') {
            steps {
                sh 'make publish'
            }
        }
    }
}
```

### Scripted pipeline

In Scripted Pipeline syntax, one or more `node` blocks do the core work throughout the entire Pipeline. Although this is not a mandatory requirement of Scripted Pipeline syntax, confining your Pipeline’s work inside of a node block does two things:

1. Schedules the steps contained within the block to run by adding an item to the Jenkins queue. As soon as an executor is free on a node, the steps will run.

2. Creates a workspace (a directory specific to that particular Pipeline) where work can be done on files checked out from source control.

```Jenkins
node { 
    stage('Build') { 
        sh 'make' 
    }
    stage('Test') {
        sh 'make check'
        junit 'reports/**/*.xml' 
    }
    if (currentBuild.currentResult == 'SUCCESS') {
        stage('Deploy') {
            sh 'make publish' 
        }
    }
}
```
