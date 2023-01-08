pipeline {
    agent {label 'DOCKER'}
    stages {
        stage('vcs') {
          steps {
        git url: 'https://github.com/spring-projects/spring-petclinic.git',
        branch: 'main'
        }
        }
      stage('jfrog') {
        steps {
        rtMavenDeployer (
        id: 'MAVEN_DEPLOYER'
        serverid: 'JFROG_JAN23'
        releaserepo: 'libs-release'
        snapshotRepo: 'libs-snapshot'
         
       )
        }
      }
      stage('maven build') {
        steps {
      rtMavenRun(
        goals: 'clean install'
        pom: 'pom.xml'
        tool: 'maven'
        deployerId: 'MAVEN_DEPLOYER'
        
        
        )
        }
      }

      stage('publish') {
         steps {
          rtPublishBuildInfo(
            serverId: 'JFROG_JAN23'
          )

        }

    }
    stage('sonar') {
        steps {
       withSonarQubeEnv('sonar') {
        sh script: 'mvn clean package sonar:sonar'
       
        }
    }

}

}

}
