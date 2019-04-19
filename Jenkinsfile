pipeline {
	agent { 
		node {
			//label 'PoseidonAgent'
			customWorkspace "C:\\NSS_${env.BRANCH_NAME}"
			
			}			
	}
	parameters {
		string(defaultValue: "0", description: 'What Pull Request ID to build?', name: 'id')
		choice(choices: ['A', 'M'], description: 'What type of build (A/M)?', name: 'type')
	}
	stages {
		stage('Installing custom tool....') {
			steps {
				tool name: 'toolbelt', type: 'com.cloudbees.jenkins.plugins.customtools.CustomTool'
			}
	}
		stage('Cleanup Working folder') {
			steps {
				dir(path: 'JenkinsAutomationBatchScripts') {
				  bat "1_CleanupWorkingFolder.bat"
				}
			}
		}
		stage('Install NPM') {
			steps {
				dir(path: 'JenkinsAutomationBatchScripts') {
				  bat "21_InstallNpm.bat"
				  bat "22_InstallSFDX.bat"
				}
			}
		}
		stage('Test deployment of current PR into target environment') {	
			when {
				not {
					anyOf {
						branch 'release*';
						branch 'master';
						branch 'develop';
						branch 'stage*';
					}
				} 
			}
			steps {
				dir(path: 'AutomationJavascripts') {
					bat "faster.bat deploy.js ${env.BRANCH_NAME}"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "3_CreateSFDXProject.bat"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "4_AuthenticateOrg.bat 3MVG9Y6d_Btp4xp4wfBKmp3vGXkapSw0_.L6VfxxPxxxx26yt8aDtjIeeRQOoHhbj9idlRG_AcS0EHGP1SB7z jayashreenaikys@salesforce.com https://login.salesforce.com"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "5_ConvertToMDAPI.bat"
				}	
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "6_DeployToOrg.bat -c -l RunLocalTests"
				}
			}
		}	
		/*stage('Deploy to Dev environment of latest closed PR') {
			when {
				branch 'develop' 
			}
			steps {
				dir(path: 'AutomationJavascripts') {
					bat "faster.bat deployLatestPRToBranch.js ${env.BRANCH_NAME} ${env.id} ${env.type}"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "3_CreateSFDXProject.bat"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "4_AuthenticateOrg.bat 3MVG9LzKxa43zqdKmeTTNjkqgDwAjZ45_9.n20NP8.nyUFLuAN0kxx_N9T6P_mNS236IoGsPjXBNpWfzFGCkq deployment.user@ef.com.lang.dev https://test.salesforce.com"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "5_ConvertToMDAPI.bat"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "6_DeployToOrg.bat"
				}
			}			
			post {
				always {
				    echo 'TODO: Run a clean up job'
				}
				success {
					dir(path: 'JenkinsAutomationBatchScripts') {
						bat "7_UpdateStatusOnGit.bat ${env.BRANCH_NAME} Deployed ${env.type} ${env.id}"
					}
				}
				failure {
					dir(path: 'JenkinsAutomationBatchScripts') {
						bat "7_UpdateStatusOnGit.bat ${env.BRANCH_NAME} Deployment_Failed ${env.type} ${env.id}"
					}
				}
			}
		}*/
		stage('Deployment of release into Prodution') {	
			when {
					branch 'stage*';
			}
			steps {
				dir(path: 'AutomationJavascripts') {
					bat "faster.bat deployLatestPRToBranch.js ${env.BRANCH_NAME} ${env.id} ${env.type}"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "3_CreateSFDXProject.bat"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "4_AuthenticateOrg.bat 3MVG9Y6d_Btp4xp4wfBKmp3vGXkapSw0_.L6VfxxPxxxx26yt8aDtjIeeRQOoHhbj9idlRG_AcS0EHGP1SB7z jayashreenaikys@salesforce.com https://login.salesforce.com"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "5_ConvertToMDAPI.bat"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "6_DeployToOrg.bat"
				}

				input message: 'Please confirm that everything in the Stage environment is working well, Proceed with deployment to Production', ok: 'It works great!'	
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "4_AuthenticateOrg.bat 3MVG9Y6d_Btp4xp4wfBKmp3vGXkapSw0_.L6VfxxPxxxx26yt8aDtjIeeRQOoHhbj9idlRG_AcS0EHGP1SB7z jayashreenaikys@salesforce.com https://login.salesforce.com"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "6_DeployToOrg.bat"
				}
			}	
			post {
				always {
				    echo 'TODO: Run a clean up job'
				}
				success {
					dir(path: 'JenkinsAutomationBatchScripts') {
						bat "7_UpdateStatusOnGit.bat ${env.BRANCH_NAME} Deployed ${env.type} ${env.id}"
					}
				}
				failure {
					dir(path: 'JenkinsAutomationBatchScripts') {
						bat "7_UpdateStatusOnGit.bat ${env.BRANCH_NAME} Deployment_Failed ${env.type} ${env.id}"
					}
				}
			}	
		}
		/*stage('Deploy to QA environment of latest closed PR') {
			when {
				branch 'release*' 
			}
			steps {
				dir(path: 'AutomationJavascripts') {
					bat "faster.bat deployLatestPRToBranch.js ${env.BRANCH_NAME} ${env.id} ${env.type}"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "3_CreateSFDXProject.bat"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "4_AuthenticateOrg.bat 3MVG9LzKxa43zqdKmeTTNjkqgDw_DQVxe8YPiNAR0uc6sIsYu_eyE0Xu7648EuymI9YOsdwZH99IqKwF8bNbn deployment.user@ef.com.lang.qa https://test.salesforce.com"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "5_ConvertToMDAPI.bat"
				}
				dir(path: 'JenkinsAutomationBatchScripts') {
					bat "6_DeployToOrg.bat"
				}
		  	}						
		post {
				always {
				    echo 'TODO: Run a clean up job'
				}
				success {
					dir(path: 'JenkinsAutomationBatchScripts') {
						bat "7_UpdateStatusOnGit.bat ${env.BRANCH_NAME} Deployed ${env.type} ${env.id}"
					}
				}
				failure {
					dir(path: 'JenkinsAutomationBatchScripts') {
						bat "7_UpdateStatusOnGit.bat ${env.BRANCH_NAME} Deployment_Failed ${env.type} ${env.id}"
					}
				}
			}		
		}*/
	}
}
