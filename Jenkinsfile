pipeline {
    agent any
    stages {
        stage ("Build Backend"){
            steps {
                bat "mvn clean package -DskipTests=true"
            }
        }

        stage("Unit Tests"){
            steps{
                bat "mvn test"
            }
        }

        // stage ('Sonar Analysis'){
        //     environment {
        //         scannerHome = = tool "SONAR_SCANNER"
        //     }
        //     steps{
        //         withSonarQubeEnv('SONNAR_LOCAL'){
        //             bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeplyBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login= -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**/Application.java "
        //         }
        //     }
        // }

        // stage ("Quality Gate"){
        //     sleep(30)
        //     steps{
        //         timeout(time: 1, unit: 'MINUTES'){
        //             waitForQualityGate abortPipeline: true
        //         }
                
        //     }
        // }
        stage("Deploy Backend"){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }

        stage("API Test"){
            steps{
                dir('api-test') {
                    git 'https://github.com/barreto-lucas/tasks-api-test'
                    bat "mvn test"
                } 
            }
        }

        stage("Build Frontend"){
            steps{
                dir('tasks'){
                    git "https://github.com/barreto-lucas/tasks-frontend"
                    bat 'mvn clean package -DskipTests=true'
                }
            }
            
        }

        stage('Deploy Frontend'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
            }
        }

    }
}

