pipeline {
    agent any

    parameters {
        string(name: 'DOCKERTAG', defaultValue: '', description: 'Docker Image Tag (Build Number)')
    }

    environment {
        GIT_CREDENTIALS_ID = 'github-id' // Your Jenkins Git credentials ID
        IMAGE_NAME = 'sahal56/netflix'
        MANIFEST_REPO = 'https://github.com/Sahal56/netflix-k8s-manifests.git'
    }

    stages {
        stage('Cloning Manifest Repo') {
            steps {
                dir('manifest') {
                    git credentialsId: "${env.GIT_CREDENTIALS_ID}", url: "${env.MANIFEST_REPO}"
                }
            }
        }

        stage('Update & Push Image Tag in Manifest Repo') {
            steps {
                dir('manifest') {
                    sh """
                        # Updating image in Kubernetes YAML
                        sed -i 's|image: ${IMAGE_NAME}:.*|image: ${IMAGE_NAME}:${DOCKERTAG}|' Kubernetes/deployment.yaml

                        # Git commit and push
                        git config user.name "sahal pathan"
                        git config user.email "sahalpathan5601@gmail.com"
                        git add Kubernetes/deployment.yaml
                        git commit -m "Done by Jenkins Job updatemanifestjob: ${env.BUILD_NUMBER}"
                        git push origin main
                    """
                }
            }
        }
    }
}
