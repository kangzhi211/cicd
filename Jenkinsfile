pipeline {
    agent any  // 在任何可用节点上运行
    environment{
        harboruser = 'admin'
        harborpasswd = 'cao123456'
        harboradrr = '10.0.0.6'
        harborrepo = 'pipe'
    }
    
    stages {
        stage('拉取代码') {  
            steps {
                git branch: 'main', url: 'https://github.com/kangzhi211/cicd.git'
            }
        }

        stage('构建项目（示例为Maven）') {  
            steps {
                sh '/var/jenkins_home/maven/bin/mvn clean   package  -DskipTests'
            }
        }

//        stage('sonar质量检测') {  // 运行测试
//            steps {
//                sh '/var/jenkins_home/sonar-scanner/bin/sonar-scanner -Dsonar.projectname=${JOB_NAME}   -Dsonar.projectKey=${JOB_NAME}   -Dsonar.sources=./ -Dsonar.java.binaries=target/   -Dsonar.login=sqa_6173a0aca6564093612f206e8fd3cfb4a5a37244'
//            }
            
//        }

        stage('构建推送镜像') {  // 部署（示例为复制文件）
            steps {
                sh '''cp target/*.jar   ./docker/
cd /var/jenkins_home/workspace/${JOB_NAME}/docker
docker build -t ${harboradrr}/${JOB_NAME}/demo:${tag}  .
docker login -u ${harboruser}  -p ${harborpasswd}  ${harboradrr}
docker push ${harboradrr}/${JOB_NAME}/demo:${tag}'''
            }
        }

        stage('目标服务器运行') {  // 运行测试
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'slb-6', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: "/test02/pull.sh  $harboradrr  $harborrepo    demo  $tag    $outport  $inport", execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
            
        }
    }

    
}
