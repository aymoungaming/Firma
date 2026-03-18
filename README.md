1. after cloning the repository, type "npm install" in the cmd inside the "back" folder
2. create a new file in the root and name it ".env"
3. paste the following content inside the ".env" file MONGO_URI=mongodb://mongo:27017/agritechDB
PORT=3000
4. type "docker-compose up --build" in the cmd  in the root
5. type "http://localhost:3000⁠" in the browser
6. test any functionality in the website, the backend must be functional
7. type "docker run -d -p 8080:8080 -p 50000:50000 --name jenkins-server -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins/jenkins:lts" in the cmd
8. open "http://localhost:8080/" and enter your credentials
9. press "new item"
10. enter "item name" then choose "pipeline"
11. change definition to "pipeline script from SCM"
12. in "SCM" choose Git
13. enter the Repository URL which is "https://github.com/aymoungaming/Firma.git"
14. change Branch Specifier to "*/main"
15. click "save"
16. type "docker exec jenkins-server apt-get update"
17. type "docker exec jenkins-server apt-get install -y docker.io libatomic1"
18. type "docker exec jenkins-server git config --global --add safe.directory "*""
19. type "docker exec jenkins-server rm -rf /var/jenkins_home/workspace/Firma"
20. type "docker exec jenkins-server rm -rf /var/jenkins_home/workspace/Firma@tmp"
21. click "Build now"
