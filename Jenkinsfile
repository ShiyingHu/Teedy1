pipeline {
agent any
stages {
        stage('Initialize') {
            steps {
                // 清理前一个构建的工作空间
                cleanWs()
            }
        }
        
        stage('Build') {
            steps {
                // 执行 Maven 构建
                sh 'mvn clean install'
            }
        }
        
        stage('Test') {
            steps {
                // 运行测试和生成 surefire 报告
                sh 'mvn test'
            }
            post {
                always {
                    // 收集测试结果
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Generate Javadoc') {
            steps {
                // 生成 Javadoc
                sh 'mvn javadoc:javadoc javadoc:jar'
            }
        }
        
        stage('Archive Artifacts') {
            steps {
                // 归档构建的工件
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                archiveArtifacts artifacts: '**/target/site/apidocs/**/*', fingerprint: true
            }
        }
    }
}
