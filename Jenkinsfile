pipeline {
    agent any
    
    
    stages {
        stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                            string(
                                defaultValue: '', 
                                name: 'BUILD', 
                            ),
							string(
                                defaultValue: '', 
                                name: 'TIME', 
                            )
                        ])
                    ])
                }
            }
		}
        stage('Ansible Deploy to prod'){
            when {
                environment name: 'DEPLOY_TO_STAGING', value: 'true'
            }
            steps {
                script {
                    try {
                        ansiblePlaybook([
                        inventory   : 'ansible/prod.inventory',
                        playbook    : 'ansible/site.yml',
                        installation: 'ansible',
                        colorized   : true,
                        credentialsId: 'applogin-prod',
                        disableHostKeyChecking: true,
                        extraVars   : [
                            USER: "admin",
                            PASS: "admin123",
                            nexusip: "172.31.4.196",
                            reponame: "vprofile-release",
                            groupid: "QA",
                            time: "${env.TIME}",
                            build: "${env.BUILD}",
                            artifactid: "vproapp",
                            vprofile_version: "vproapp-${env.BUILD}-${env.TIME}.war"
                            ]
                        ])
                    } 
                    catch (Exception e) {
                        echo "Ansible deployment failed: ${e.getMessage()}"
                        currentBuild.result = 'UNSTABLE'
                    }
                }
            }
        }


          
		  

          
        
    }
}
