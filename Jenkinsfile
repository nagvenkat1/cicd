pipeline {
    agent {label 'key'}
    stages {
        stage('vcs') {
          steps {
        git url: 'https://github.com/spring-projects/spring-petclinic.git',
        branch: 'main'
        }
        }
        stage('mvn path') {
          steps {
               sh 'export PATH=$PATH/usr/share/maven/bin:$PATH'
        }
        }
        stage('maven build') {
            steps {
                rtMavenRun (
                goals: 'package',
                pom: 'pom.xml',
                tool: 'Apache Maven 3.6.3',
                deployerId: 'Apache Maven 3.6.3'
                
                )
        }
      }
      stage('jfrog') {
         steps {
            rtMavenDeployer (
            id: 'MAVEN',
            serverId: 'JFROG_JAN23',
            releaseRepo: 'libs-release/',
            snapshotRepo: 'libs-snapshot/'
            
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
