pipeline {
    agent { 
        docker {
            image 'maven:3.3.3' 
            label 'master'
            args '-v /root/.m2:/root/.m2'
        }
    }
    
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite1'
    }
    
    stages {
        stage('mvn test') {
            steps {
                sh "mvn test"
            }
        }
        
        stage('Parallel Stage') {
            parallel {
                stage('Branch A') {
                    steps {
                        echo "On Branch A"
                    }
                }
                stage('Branch B') {
                    steps {
                        echo "On Branch B"
                    }
                }
            }
        }
        
        //stage('Sanity check') {
        //    steps {
        //        input "Does the staging mvn test ok?"
        //    }
        //}
        
        stage('mvn build') {
            steps {
                //mvn构建
                sh "mvn install -Dmaven.test.skip=true"
            }
        }
        
        stage('deploy') {
            steps {
                //执行部署脚本
                echo "deploy ......"
                sh '''
                    echo "deploy1 ......"
                    echo "deploy1 ......"
                    echo "deploy1 ......"
                    echo "deploy1 ......"
                '''
            }
        }
    }
    
    post {
        always {
            junit 'target/surefire-reports/TEST-*.xml'
            archive 'target/*.jar'
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
