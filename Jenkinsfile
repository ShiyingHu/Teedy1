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
                // 构建项目，跳过测试
                sh 'mvn -B -DskipTests clean package'
            }
        }
        
        stage('Test') {
            steps {
                // 运行测试并生成 Surefire 报告
                sh 'mvn test surefire-report:report'
            }
            post {
                always {
                    // 收集 Surefire 测试结果
                    junit '**/target/surefire-reports/*.xml'
                    // 归档 Surefire 报告
                    archiveArtifacts artifacts: '**/target/site/surefire-report.html', fingerprint: true
                }
            }
        }
        
        stage('Generate Javadoc') {
            steps {
                // 生成 Javadoc 并打包为 JAR
                sh 'mvn javadoc:javadoc javadoc:jar'
            }
            post {
                always {
                    // 归档生成的 Javadoc
                    archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                }
            }
        }
        
        stage('Archive Artifacts') {
            steps {
                // 归档构建产物
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
}
