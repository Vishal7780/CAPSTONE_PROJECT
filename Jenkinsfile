pipeline {
    agent any

     environment{
       registryCredential = 'ecr:us-east-1:AWSCRED'
       appRegistry = "258535055782.dkr.ecr.us-east-1.amazonaws.com/capstoneprojectrepository"
       capstoneRegistry = "https://258535055782.dkr.ecr.us-east-1.amazonaws.com"
       cluster = "capstoneprojectcluster"
        service = "capstoneprojectservice"
   }

    stages {
        stage('Clone Website') {
            steps {
                git url:'https://github.com/Vishal7780/CAPSTONE_PROJECT'
            }
        }

       stage("Docker Build Image"){
           steps{
             script{
                dockerImage = docker.build(appRegistry+ ":$BUILD_NUMBER",".")
             }
           }
      }

       stage('Test Website') {
            steps {
                // Run tests on the website
                sh 'echo "Running tests on the website"'
            }
       }

       stage("Upload App Image"){
         steps{
            script{
                docker.withRegistry(capstoneRegistry, registryCredential){
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                }
            }
         }
      }

      stage('Deploy to ecs'){
         steps{
            withAWS(credentials: '<CredentialID>', region: 'us-east-1'){
                sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
            }
         }
      }

    }
}
