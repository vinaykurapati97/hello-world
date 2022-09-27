pipeline{
    agent any

    tools {
         maven 'M2_HOME'
         jdk 'JAVA_HOME'
    }
    
  stages {
        
    stage('Git checkout') {
      steps {
        git 'https://github.com/1919teja/hello-world.git'
      }
    }
     
    stage('build war file ') {
      steps {
        sh ' mvn clean install '
      } 
    }
     
	stage(' check ssh connection of deployment servers ') {
      steps {
         sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible -m ping all', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target/', sourceFiles: 'webapp/target/**.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
         
      }
    }
    
    stage(' Build Docker image and push to Docker hub ') {
      steps {
         sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook  /opt/docker/regapp.yml;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target/', sourceFiles: 'webapp/target/**.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
    
	stage('docker container deployment') {
             steps {
         sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sleep 30;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target/', sourceFiles: 'webapp/target/**.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
    
    stage(' deploy Docker container on tomcat ') {
      steps {
         sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook  /opt/docker/deploy_docker.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target/', sourceFiles: 'webapp/target/**.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
    
    
    }
  }
