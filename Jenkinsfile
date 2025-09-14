pipeline {
    agent any  // 在任何可用节点上运行

    stages {
        stage('Checkout') {  // 拉取代码
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/example.git'
            }
        }

        stage('Build') {  // 构建项目（示例为Maven）
            steps {
                sh 'mvn clean package'  // 如果是Node.js可替换为 `npm install`
            }
        }

        stage('Test') {  // 运行测试
            steps {
                sh 'mvn test'  // 或 `npm test` / `pytest` 等
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'  // 收集测试报告
                }
            }
        }

        stage('Deploy') {  // 部署（示例为复制文件）
            steps {
                sh 'cp target/*.war /opt/tomcat/webapps/'  // 根据实际需求修改
            }
        }
    }

    
}
