pipeline{
    agent any
    stages{
        stage('SCM checkout'){
            steps{
                git 'https://github.com/Sidkgupta/maven-multi-module-example.git'
                
            }
            
        }
        stage('Build'){
            steps{
                sh '/usr/share/maven/bin/mvn clean package -Dmaven.test.skip=true'
            }
        }
    stage('Kindly Provide Approval for Deployment'){

       steps{

  slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#comp', color: 'good', message: "${env.BUILD_URL}",teamDomain: 'sidteam', tokenCredentialId: 'slackid'
   
   script{

                def userInput

  try {

    userInput = input(

        id: 'Proceed1', message: 'UAT Deployment Approval',submitterParameter: 'submitter',submitter:'approver', parameters: [

        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please Confirm you agree with this']

        ])

} catch(err) {

    def user = err.getCauses()[0].getUser()

    userInput = false

    echo "Aborted by: [${user}]"

}

}
 }

 }  
        
      stage('release'){
           when {
                branch 'release'
            }
            steps{
        
        mvn build-helper:parse-version versions:set -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.nextIncrementalVersion} versions:commit
            }
      }
        
        
        
        stage('push changes to master'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'gitid', passwordVariable: 'Pass', usernameVariable: 'User')]) {
    
                     sh 'git push --all'
}
               
    }
    }    
    }
}
