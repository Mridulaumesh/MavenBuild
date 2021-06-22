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
                script{
                def Sonar_path = tool name: 'sonar462', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                
                }
            
        }
	

	
	
	stage('step :4 docker image build'){
                script{
                sh "docker build -t mridulaumesh03g/java-maven:v1.0 ."
            }
        }
	stage('step:5 docker image push'){
                withCredentials([string(credentialsId: 'docker_cred', variable: 'DockerPassword')]) {
                sh "docker login -u mridulaumesh03g -p esh03g0A@ecw6u"
                sh "docker push mridulaumesh03g/java-maven:v1.0"
                }
               
            
        }
}
