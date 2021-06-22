node('master') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage('step:3 sonar scanner'){
            steps{
                script{
                def Sonar_path = tool name: 'sonar462', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                
                }
            }
        }

	
	
	stage('step :4 docker image build'){
            steps{
                sh "docker build -t mridulaumesh03g/java-maven:v1.0 ."
            }
        }
	stage('step:5 docker image push'){
            steps{
                withCredentials([string(credentialsId: 'docker_cred', variable: 'DockerPassword')]) {
                    sh "docker login -u mridulaumesh03g -p ${DockerPassword}"
                    sh "docker push mridulaumesh03g/java-maven:v1.0"
                }
               
            }
        }
}
