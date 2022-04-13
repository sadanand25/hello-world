def remote = [:]
remote.name = 'kubemaster'
remote.host = '192.168.56.2'
remote.user = 'vagrant'
remote.password = 'vagrant'
remote.allowAnyHosts = true
sudo = true
pipeline {
    agent any
  
    stages {
        stage("Git Clone"){
            steps {
            git branch: 'master', credentialsId: 'github', url: 'https://github.com/sadanand25/hello-world.git'
            }
        }
        stage('Build') {
           steps{
        // Run the maven build
              withEnv(["M2_HOME=/opt/apache-maven-3.8.5"]) {
                sh '"$M2_HOME/bin/mvn" -Dmaven.test.failure.ignore clean install'

            }
          }
		}
        stage("Docker create and Push images"){
           steps{
              sshPublisher(publishers: [sshPublisherDesc(configName: 'kubemaster', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo ansible-playbook /opt/docker/dockerimagepush.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        stage("SSH Into kubemaster Server"){
            stages{                
                stage("Deploying application on K8s cluster"){
                    steps{
                        sshCommand remote: remote, command: "sudo ansible-playbook /opt/docker/kubernet_deploy.yml"
                    }
            }
        }
      }
    }

}
