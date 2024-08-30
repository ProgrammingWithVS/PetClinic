pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven'
    }

    stages {
        stage('Download') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/ProgrammingWithVS/PetClinic.git'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

    }
}
