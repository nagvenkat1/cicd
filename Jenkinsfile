pipeline {
    agent {label 'key'}
    stages {
        stage('vcs') {
          steps {
        git url: 'https://github.com/spring-projects/spring-petclinic.git',
        branch: 'main'
        }
        }
    stage('maven build') {
        steps {
      rtMavenRun (
        goals: 'clean install',
        pom: 'pom.xml',
        tool: 'MAVEN',
        deployerId: 'MAVEN'
        
        )
        }
      }  
        
      stage('jfrog') {
        steps {
        rtMavenDeployer (
        id: 'MAVEN',
        serverId: 'JFROG_JAN23',
        releaseRepo: 'libs-release',
        snapshotRepo: 'libs-snapshot'
         
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
