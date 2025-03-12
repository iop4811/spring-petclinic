pipeline {
    agent any
    
    tools {
        maven "M3"
        jdk "JDK17"
    }
    
    stages {
        stage('Git Clone') {
            steps {
                echo 'Git Clone'
                git url: 'https://github.com/iop4811/spring-petclinic.git',
                    branch: 'main'
            }
            post {
                success {
                    echo 'Git Success'
                }
                failure {
                    echo 'Git Clone Fail'
                }
            }
        }
        
        stage('Maven Build') {
            steps {
                echo 'Maven Build'
                sh 'mvn -Dmaven.test.failure.ignore=true clean package'
            }
        }
        stage('SSH Pulish') {
            steps {
                echo 'SSH Pulish'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'target',
                transfers: [sshTransfer(cleanRemote: false, excludes: '',
                execCommand: '''fuser -k 8080/tcp
                export BUILD_IDPetClinic
                nohup java -jar spring-petclinic-3.4.0-SNAPSHOT.jar >> nohup.out 2<&1 & ''',
                execTimeout: 120000, 
                flatten: false, makeEmptyDirs: false, 
                noDefaultExcludes: false, patternSeparator: '[, ]+', 
                remoteDirectory: '', 
                remoteDirectorySDF: false, 
                removePrefix: 'target', 
                sourceFiles: 'target/*.jar')],
                usePromotionTimestamp: false, 
                useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        
    }
}
