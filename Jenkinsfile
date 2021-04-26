pipeline {
    agent any
    tools {
        maven 'Maven 3'
        jdk 'JDK8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage('Build') {
            steps {
                sh './waterfall build'
            }
        }
        stage('Results') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
                archive 'target/*.jar'
            }
        }
        stage('Deploy') {
            steps {
                configFileProvider([configFile(fileId: 'b8e2316f-5cad-48e6-ab64-1afae45b71b3', variable: 'MAVEN_SETTINGS')]) {
                    sh 'mvn -s $MAVEN_SETTINGS -DrepositoryId=maven-releases deploy'
                }
            }
        }
    }
}