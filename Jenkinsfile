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
        stage('Sonar') {
            steps {
                withSonarQubeEnv('sonar') {
                sh 'mvn clean package sonar:sonar'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy to Nexus') {
            steps {
                configFileProvider([configFile(fileId: 'anything', variable: 'mavensettings')]) {
                    sh "mvn -s $mavensettings clean deploy"
                }
            }
        }
        stage('Docker Build') {
            steps {
                withCredentials([string(credentialsId: 'dockertoken', variable: 'dockertoken')]) {
                    sh 'docker login -u sucheeth -p $dockertoken'
                    sh 'docker build -t petclinicnexus .'
                    sh 'docker tag petclinicnexus sucheeth/petclinicnexus:latest'
                    sh 'docker push sucheeth/petclinicnexus:latest'                    
                }
            }
        }
        stage('Docker Deploy') {
            steps {
                withCredentials([string(credentialsId: 'dockertoken', variable: 'dockertoken')]) {
                    sh 'docker run -d --name petclinic -p 5050:5050 sucheeth/petclinicnexus:latest'
                }
            }
        }
    }
}
