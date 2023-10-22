pipeline {
    agent any
      
               
       stages {
            stage('SCM checkout') {
                  steps {
                        git branch: "main", credentialsId: '07d001cf-30bc-4081-91a1-a096fa7236a0', url: 'https://github.com/.git'
                        }
             }
             
              
              stage('transfer artifacts') {
                    steps {
                          sshPublisher(publishers: [sshPublisherDesc(configName: 'webserver', transfers: [sshTransfer(excludes: '', execCommand: '', execTimeout: 120000, flatten: true, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/var/www/html/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
                          }
              }
       }
}
