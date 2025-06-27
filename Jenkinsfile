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
                    git branch: 'main', credentialsId: "${env.GIT_CREDENTIALS_ID}", url: "${env.MANIFEST_REPO}"
                }
            }
        }

        stage('Update & Push Image Tag in Manifest Repo') {
            steps {
                dir('manifest') {
                    withCredentials([usernamePassword(credentialsId: 'github-id', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh """
                        # Update image tag in YAML
                        sed -i 's|image: ${IMAGE_NAME}:.*|image: ${IMAGE_NAME}:${DOCKERTAG}|' Kubernetes/deployment.yaml
                        # Git commit
                        git config user.name "sahal pathan"
                        git config user.email "sahalpathan5601@gmail.com"
                        git add Kubernetes/deployment.yaml
                        git commit -m "Done by Jenkins Job UpdateManifest-CD-Pipeline: ${env.BUILD_NUMBER}" || echo "No changes to commit"

                        # Push using injected credentials
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Sahal56/netflix-k8s-manifests.git main
                        """
                }
                }
            }
        }
        
    }
}
