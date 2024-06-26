pipeline{
 agent any
   options {
	  skipDefaultCheckout()
	}
 environment {
	   BLD_STATUS = '$BUILD_STATUS'
	   PRJ_NAME = '$PROJECT_NAME'
 }// end of environment
 
 stages{
	  stage('CleanWorkspace') {
            steps {
                cleanWs()
            }
        }
   stage("Checkout Source"){
     steps{
	    checkout(
                    changelog: false,
                    poll: false,
                    scm: [
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'RelativeTargetDirectory', relativeTargetDir: '.']
                        ],
                        gitTool: 'Default',
                        submoduleCfg: [],
                        userRemoteConfigs: [
                            [
                                url: 'https://github.com/venh/simple-hello-world-java.git'				    
                            ]
                        ]
                    ]
                )
	 }//end of steps
   }//end of stage 
   stage("Compile"){
     steps{
	   echo "Compilation..."
	    script{
			sh "javac HelloWorld.java"
		}//end of script
	 }//end of steps
   }//end of stage
 }//end of stages
 post{
      success {
         script {
	    try {	 
	       sh "java HelloWorld"
	       throw new Exception("Something went wrong!")
	    } catch(Exception e){
		    echo 'Exception occurred: ' + e.toString()
                    currentBuild.result = 'FAILURE'
	    }
	 }
      }
      always{		
	  //sendEmail('$DEFAULT_RECIPIENTS', null, "[${env.BLD_STATUS}] - ${env.PRJ_NAME} - Build # ${BUILD_NUMBER} ($BUILD_ID)", '''${SCRIPT, template="custom-html.template"}''') 
	  printMsg("$JOB_BASE_NAME")
        echo "Always executes..."
	  emailext body: '${SCRIPT, template="custom-html.template"}', subject: 'Build Completed', to: 'venkatesh.subramanian@xoriant.com'
      }
    } // end of post
}//end of Pipeline
