pipeline{
    agent any
    environment {
        PROJECT_ID = 'plasma-ivy-373905 '
        CLUSTER_NAME = 'cluster-1 '
        LOCATION = 'us-central1-c 	'
        CREDENTIALS_ID = 'My Project 16902'
        DOCKERHUB_CREDENTIALS = credentials('docker')
    }
stages {
        stage('Build App'){
            steps{
                sh './mvnw clean package'
            }
        post{
            success{
                junit 'target/surefire-reports/*.xml'
                archiveArtifacts "target/*.jar"
            } 
        }    
        }
       stage('Docker build'){
        steps{
            script{
                sh 'docker build -t moredatta574/petclinic:0.1 . '
            }
        }
       }
       stage('Login') {
		    steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_USR'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW'
		        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
			}
		}


       stage('Deploy to kubernetes'){
        steps{
            
            step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'petclinic.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
		   echo "Deployment Finished ..."
        }
       }
}

    
}
