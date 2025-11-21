pipeline {

    agent any
 
  
    stages {
 
        stage('Checkout Code') {

            steps {

                echo "🔹 Checking out repository..."

                git branch: 'main', url: 'https://github.com/mohamed55979/HelloApp'

            }

        }
 
        stage('Terraform Init') {

            steps {

                echo "🔹 Initializing Terraform..."

                sh 'terraform init -reconfigure'

            }

        }
 
        stage('Terraform Plan') {

            steps {

                echo "🔹 Creating Terraform plan..."

                sh 'terraform plan -out=tfplan'

            }

        }

        stage('Import Existing Resources') {

            steps {

                echo "🔹 Importing existing node groups if needed..."

                script {
                    // Try to import node-group-2 if it exists but not in state
                    sh '''
                        if aws eks describe-nodegroup --cluster-name my-eks-project-dev-cluster --nodegroup-name my-eks-project-dev-node-group-2 --region us-east-1 2>/dev/null; then
                            echo "Node group 2 exists in AWS, checking if in state..."
                            terraform import 'module.eks.aws_eks_node_group.main[1]' my-eks-project-dev-cluster:my-eks-project-dev-node-group-2 || echo "Already in state or import failed"
                        fi
                    '''
                }

            }

        }

        stage('Terraform Apply') {

            steps {

                echo "🔹 Applying Terraform..."

                sh 'terraform apply -auto-approve tfplan'

                echo "✅ Infrastructure deployed successfully!"

            }

        }

/*
        stage('Terraform Destroy') {

            steps {

                echo "🗑️ Destroying Terraform infrastructure..."

                sh 'terraform destroy -auto-approve'

                echo "🔥 Infrastructure destroyed successfully!"

            }

        }
*/

    }
 
    post {

        success {

            echo "🎉 Pipeline completed successfully!"

        }

        failure {

            echo "❌ Pipeline failed!"

        }

    }

}
