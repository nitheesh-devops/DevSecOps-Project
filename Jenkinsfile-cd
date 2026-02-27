@Library('shared-lib') _
pipeline {
    agent any
    stages {
          stage('Notify Slack') {
            steps {
                echo "Pipeline started."
                slackNotify(
                    status: 'STARTED',
                    color: '#439FE0',
                    message: 'Pipeline execution has started'
                )
            }
        }
        stage('Checkout') {
            steps {
                echo "Cloning Repository..."
                git branch: 'master',
                    url: 'https://github.com/airtel-org/DevSecOps.git'
            }
        }



 stage('Setup KubeConfig') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-cred']]) {
                    sh '''
                        echo "Setting up kubeconfig for EKS cluster..."

                        # Export AWS credentials explicitly
                        export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                        export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                        export AWS_DEFAULT_REGION=ap-south-1

                        echo "Verifying AWS Identity..."
                        aws sts get-caller-identity

                        echo "Updating kubeconfig for EKS cluster..."
                        aws eks update-kubeconfig --region ap-south-1 --name my-ekscluster

                        echo "Kubeconfig setup complete."
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to EKS..."
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-cred']]) {
                    sh '''
                        kubectl apply -f springBootMongo.yml --validate=false
                    '''
                }
            }
        }

        stage('Verify Pods and Services') {
            steps {
                echo "Verifying Kubernetes resources..."
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-cred']]) {
                    sh '''
                        kubectl get pods -o wide
                        kubectl get svc -o wide
                    '''
                }
            }
        }

        stage('Trivy Cluster Scan') {
            steps {
                echo "Scanning Kubernetes cluster for vulnerabilities..."
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-cred']]) {  
                    sh '''
                        export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                        export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                        export AWS_DEFAULT_REGION=ap-south-1

                        echo "Running Trivy Kubernetes cluster scan..."
                        trivy k8s --report summary cluster || true
                    '''
                }
            }
        }
    }

    post {
       
        success {
            echo "Pipeline completed successfully."
             slackNotify(
            status: 'SUCCESS',
            color: 'good',
            message: 'Pipeline completed successfully'
        )
        }
        failure {
            echo "Pipeline failed. Please check the logs for details."
             slackNotify(
            status: 'FAILED',
            color: 'danger',
            message: 'Pipeline failed'
        )
        }
    }
}
