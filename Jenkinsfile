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

	stage ('Sonar Analysis'){
		sh 'mvn sonar:sonar -Dsonar.host.url=http://52.90.114.217:9000 -Dsonar.login=3f2d512066c968a048575e6a0001e6bcffe61601'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		//sh 'cp target/*.war /opt/tomcat8/webapps'
	}
	stage ('Notification'){
		//slackSend color: 'good', message: 'Deployment Sucessful'
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "anuj_sharma401@yahoo.com"
		    )
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
