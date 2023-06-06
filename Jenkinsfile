pipeline
{

    options
    {
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
    }

    agent any

    tools
    {
        maven 'maven3'
    }

    stages
    {


        stage('Check versions')
        {
            steps
            {
                echo 'checking java version'
                sh 'java --version'
				        echo 'java version is'
                 echo 'checking git version'
                sh 'git --version'
				        echo 'git version is'
                 echo 'checking maven version'
                sh 'mvn --version'
				        echo 'maven version is'
            }
        }

        stage('Code Compile')
        {
            steps
            {
                  echo 'code compilation is starting'
                  sh 'mvn clean compile'
				  echo 'code compilation is completed'
             }
        }


        stage('Sonarqube scanning')
	{
		steps{
			echo 'sonarqube'}
        }

        stage('Code Package')
        {
            steps
            {
                     echo 'code packing is starting'
                     sh 'mvn clean package'
        			 echo 'code packing is completed'
            }
        }


        stage('Build & Tag Docker Image')
        {
            steps
            {
                      echo 'Starting Building Docker Image'
                      sh 'docker build -t kulkarnisakshi/fullstackapp .'
                      sh 'docker build -t fullstackapp .'
                      echo 'Completed  Building Docker Image'
            }
        }


        stage('Docker Image Scanning')
        {
            steps
            {
                       echo 'Docker Image Scanning Started'
                       sh 'java --version'
                       echo 'Docker Image Scanning Completed'
            }
        }


        stage(' Docker Image push to Docker Hub')
        {
            steps
            {
                    script
                    {
                         withCredentials([string(credentialsId: 'dockerhub-cred', variable: 'dockerhub-cred')])
                         {
                         sh 'docker login docker.io -u kulkarnisakshi -p dockerhub'
                         echo "Push Docker Image to DockerHub : In Progress"
                         sh 'docker push kulkarnisakshi/fullstackapp:latest'
                         echo "Push Docker Image to DockerHub : In Progress"
                         sh 'whoami'
                         }
                    }
            }
        }

        stage(' Docker Image Push to Amazon ECR')
        {
            steps
            {
                script
                {
                          docker.withRegistry('https://152434801645.dkr.ecr.ap-south-1.amazonaws.com/fullstackapp', 'ecr:ap-south-1:ecr-cred')
                          {
                          sh """
                          echo "List the docker images present in local"
                          docker images
                          echo "Tagging the Docker Image: In Progress"
                          docker tag fullstackapp:latest 152434801645.dkr.ecr.ap-south-1.amazonaws.com/fullstackapp:latest
                          echo "Tagging the Docker Image: Completed"
                          echo "Push Docker Image to ECR : In Progress"
                          docker push 152434801645.dkr.ecr.ap-south-1.amazonaws.com/fullstackapp:latest
                          echo "Push Docker Image to ECR : Completed"
                          """
                          }
                }
            }
        }


  }
}
