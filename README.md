1. after cloning the repository, type "npm install" in the cmd inside the "back" folder
2. create a new file in the root and name it ".env"
3. paste the following content inside the ".env" file MONGO_URI=mongodb://mongo:27017/agritechDB
PORT=3000
4. type "docker-compose up --build" in the cmd  in the root
5. type "http://localhost:3000⁠" in the browser
6. test any functionality in the website, the backend must be functional
7. create a jenkins file besides the root
8. paste the following content inside the jenkins file :
9. pipeline {
    agent any 
    tools {
        nodejs 'Node20' 
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling the latest marketplace code from GitHub...'
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing Node.js backend packages...'
                // Tell Jenkins to go into the correct folder first
                dir('firma/Back') {
                    sh 'npm install' 
                }
            }
        }

        stage('Security Check') {
            steps {
                echo 'Running a quick check for vulnerable packages...'
                // We also need to be in that folder for npm audit!
                dir('Firma-main/Back') {
                    sh 'npm audit --audit-level=high || true'
                }
            }
        }

        stage('Test') {
            steps {
                echo 'No automated tests written yet. Skipping test phase safely!'
            }
        }

        stage('Docker Build (Dry Run)') {
            steps {
                echo 'Building the Docker image to ensure the Dockerfile works...'
                dir('firma') {
                    // We will tag (-t) the image as "marketplace-app"
                    sh 'docker build -t marketplace-app .'
                }
            }
        }

        stage('Deploy Locally') {
            steps {
                echo 'Deploying the new marketplace container locally...'
                
                // 1. Stop and remove the old version of the website (if it's running)
                // The '|| true' tells Jenkins not to panic and fail if the container doesn't exist yet
                sh 'docker rm -f my-marketplace-website || true'
                
                // 2. Start the brand new version we just built!
                // REPLACE "3000:3000" with whatever port your Node.js backend uses
                sh 'docker run -d -p 3001:3000 --name my-marketplace-website marketplace-app'
            }
        }
    }
} 
10. type "docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --restart=on-failure jenkins/jenkins:lts" in the cmd in the new outer root
11. open "http://localhost:8080/" and enter your credentials
12. press "new item"
13. enter "item name" then choose "pipeline"
14. change definition to "pipeline script from SCM"
15. in "SCM" choose Git
16. enter the Repository URL which is "https://github.com/aymoungaming/Firma.git"
17. change Branch Specifier to "*/main"
18. click "save"
19. click "Build now"
