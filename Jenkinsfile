
node("slave"){
     
         def mavenHome
        def mavenCMD
        def docker
        def dockerCMD
        def tagName = "3.3"
        
//         def dockerHubPwd = "Edureka@1992"
        
//           environment { 

//         registry = "PoojaMishra084/Certificationproj1" 

//         registryCredential = 'devopslearner'

//         dockerImage = "poojamishra084/addressbook:${tagName}"

//     }
     
        stage('Preparation of Jenkins'){
             
               echo "Setting up the Jenkins environment..."
                mavenHome = tool name: 'maven-3', type: 'maven'
                mavenCMD = "${mavenHome}/bin/mvn"
                docker = tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
                dockerCMD = "$docker/bin/docker"
               
        }

       stage('git checkout'){
         
                echo "Checking out the code from git repository..."
                git 'https://github.com/PoojaMishra084/Certificationproj1.git'
        }
           

        stage('Build, Test and Package'){
            
                echo "Building the application..."
                sh "${mavenCMD} clean package"
         }
    
        stage('Sonar Scan'){
            
               echo "Scanning application for vulnerabilities using Sonar..."
              //  sh "${mavenCMD} sonar:sonar -Dsonar.host.url=http://35.188.131.222:9000  -Dsonar.login=03c8b31da2e09c29b8eb5078385d4eeff321735d"      
        }
        
        stage('Code Coverage Analysis'){
            echo 'running code coverage analysis'
            //sh "${mavenCMD} cobertura:cobertura"
        }
        
        stage('static code analysis'){
            echo 'running static code analysis'
           // sh "${mavenCMD} pmd:pmd"
        }

        stage('Publish Report'){
   
                echo " Publishing HTML report.."
              //  publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site/', reportFiles: 'surefire-report.html', reportName: 'HTML Report', reportTitles: ''])
            
        }

        stage('Build Docker Image'){
            
                echo "Building docker image for application ..."
                sh " sudo ${dockerCMD} build -t poojamishra084/addressbook:${tagName} ."
        }
     
        stage("Push Docker Image to DockerHub"){
                echo "Log into the dockerhub and Pushing image"
//                 docker.withRegistry( '', registryCredential ) { 
//                         echo "LoggedIn successfully"
//                          dockerImage.push() 
                withCredentials([string(credentialsId: 'dockerPwd', variable: 'dockerHubPwd')]) {
                sh " sudo ${dockerCMD} login -u poojamishra084 -p ${dockerHubPwd}"
                sh " sudo ${dockerCMD} push poojamishra084/addressbook:${tagName}"
                }
        }
    
//         stage('Configure Server using Ansible'){
          
//               echo 'configuring servers'
//               sh 'ansible-playbook playbook2.yml'
   
//         }
     
     stage('Deploy Application'){
                echo 'Deploying Application on aws Instance'
               sh "sudo ${dockerCMD} run d -p 8082:8080 --name=addressbook poojamishra084/addressbook:${tagName}"
//               sshagent(['aws-ubuntu']) {
//                    sh "ssh -o StrictHostKeyChecking=no ${user}@${ipAddress} ${dockerRun}" 
               }
            }  
    
        stage('Workspace Cleanup'){
           
                echo "Clean the Jenkin Pipeline's workspace..."
              //  cleanWs()
          
         
       }
}
