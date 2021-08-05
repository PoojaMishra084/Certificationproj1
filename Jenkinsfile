
node("slave"){
     
        def mavenHome
        def mavenCMD
        def docker
        def dockerCMD
        def tagName = "5"
        
     
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
    
       
        
        stage('Code Coverage Analysis'){
            echo 'running code coverage analysis'
            sh " sudo ${mavenCMD} cobertura:cobertura"
        }
        
        stage('static code analysis'){
            echo 'running static code analysis'
           sh " sudo ${mavenCMD} pmd:pmd"
        }

        stage('Publish Report'){
   
                echo " Publishing HTML report.."
               publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site/', reportFiles: 'surefire-report.html', reportName: 'HTML Report', reportTitles: ''])
            
        }

        stage('Build Docker Image'){
            
                echo "Building docker image for application ..."
                sh " sudo ${dockerCMD} build -t poojamishra084/addressbook:${tagName} ."
        }
     
        stage("Push Docker Image to DockerHub"){
                echo "Log into the dockerhub and Pushing image"
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
                echo 'Deploying Application on aws instance made as jenkins slave machine'
               sh "sudo ${dockerCMD} run -p 8082:8080 -d poojamishra084/addressbook:${tagName}"
//               sshagent(['aws-ubuntu']) {
//                    sh "ssh -o StrictHostKeyChecking=no ${user}@${ipAddress} ${dockerRun}" 
              // }
            }  
    
        stage('Workspace Cleanup'){
           
                echo "Clean the Jenkin Pipeline's workspace..."
                cleanWs()
          
         
       }
}
