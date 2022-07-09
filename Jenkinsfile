pipeline {
    agent any
    tools {
        maven 'maven_3_8_6'
    }
    stages {
        
        stage('Kitchen') {
            when {
                anyOf {
                    changeset "*kitchen-service/**"
                    expression { currentBuild.previousBuild.result != "SUCCESS"}
                }
            }
            steps {
                dir("kitchen-service/"){
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker_hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                        sh 'docker login -u $USERNAME -p $PASSWORD'
                        sh "docker build -t juanddtr/kitchen-service:latest ."
                        sh 'docker push juanddtr/kitchen-service:latest'
                    }
                }
            }
        }

        stage('Order') {
            when {
                anyOf {
                    changeset "*order-service/**"
                    expression { currentBuild.previousBuild.result != "SUCCESS"}
                }
            }
            steps {
                dir("order-service/"){
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker_hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                        sh 'docker login -u $USERNAME -p $PASSWORD'
                        sh "docker build -t juanddtr/order-service:latest ."
                        sh 'docker push juanddtr/order-service:latest'
                    }
                }
            }
        }

        stage('Payment') {
            when {
                anyOf {
                    changeset "*payment-service/**"
                    expression { currentBuild.previousBuild.result != "SUCCESS"}
                }
            }
            steps {
                dir("payment-service/"){
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker_hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                        sh 'docker login -u $USERNAME -p $PASSWORD'
                        sh "docker build -t juanddtr/payment-service:latest ."
                        sh 'docker push juanddtr/payment-service:latest'
                    }
                }
            }
        }

        stage('Database') {
            steps {
                dir("liquibase/"){
                    sh '/opt/liquibase/liquibase --changeLogFile="changesets/db.changelog-master.xml" update'
                }
            }
        }
    }
}