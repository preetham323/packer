pipeline {
  agent any
   stages {
       stage("aws jenkins packer") {
         steps {
                  withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws_credential',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                                 ]]) {
                    sh "aws s3 ls"
                }
               }
       }
         stage ('Packer init') {
                  steps {
                   script {
                    echo "initializing packer"
                    sh "/usr/local/bin/packer version"
                    sh  "/usr/local/bin/packer init aws-ami-v1.pkr.hcl"
                      }
                }
        }
          stage ('Packer validate') {
                   steps {
                    echo 'validating packer code'
                    sh '/usr/local/bin/packer validate aws-ami-v1.pkr.hcl'
                }
             }
          stage ('packer build ami') {
                    steps {
                        withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws_credential',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                                 ]]) {
                    echo 'building ami'
                    sh '/usr/local/bin/packer build aws-ami-v1.pkr.hcl'
              }
        }
  }
   }
}

