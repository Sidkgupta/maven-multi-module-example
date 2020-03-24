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
    stage('Kindly Provide Approval for UAT Deployment'){

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
                sh '/usr/share/maven/bin/mvn --batch-mode release:clean release:prepare release:perform -DreleaseVersion=2.0 -Ddevelopmentversion=1.9-SNAPSHOT'
            }
        }
        stage('deploy'){
            steps{
                sh '/usr/share/maven/bin/mvn clean deploy -Dmaven.test.skip=true'
                
    }    
    
}
    }
}
