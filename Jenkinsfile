pipeline {
    agent {label 'key'}
    stages {
      stage('vcs') {
        steps {
            git url: 'https://github.com/nagvenkat1/spring-petclinic.git',
            branch: 'main'
        }
        }
         stage('jfrog') {
         steps {
            rtMavenDeployer (
            id: 'Maven_Deployer',
            serverId: 'JFROG_JAN23',
            releaseRepo: 'libs-release-local',
            snapshotRepo: 'libs-snapshot-local'
            )
         }
         }
        stage('maven run') {
          steps {
            rtMavenRun (
              goals: 'clean install',
              pom: 'pom.xml',
              tool: 'maven',
              deployerId: 'Maven_Deployer'
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
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: "HOURS") {
                waitForQualityGate abortPipeline: true
              }
            }
          }
        
       }
}
