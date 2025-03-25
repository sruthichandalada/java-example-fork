pipeline{
    agent {label 'build'}
    stages{
       stage('Git Checkout Stage'){
            steps{
                git branch: 'main', url: 'https://github.com/sruthichandalada/java-example-fork.git'
            }
         }        
       stage('Build Stage'){
            steps{
                sh 'mvn clean install'
            }
         }
        stage('SonarQube Analysis Stage') {
            steps{
                withSonarQubeEnv('sonarqube-server') { 
                    sh "mvn clean verify sonar:sonar"
                }
            }
        } 
           stage('Deploy to Tomcat') {
             steps {
               script {
                 def warFile = "target/works-with-heroku-1.0.war"  // Adjust based on actual .war file
                 def tomcatUser = "root"  // Linux user on the Tomcat server
                 def tomcatHost = "3.110.135.141"  // Tomcat server IP or hostname
                 def tomcatPath = "/opt/tomcat/apache-tomcat-9.0.98/webapps"  // Path to Tomcat's webapps directory

              // Copy WAR file to Tomcat server
              sh "scp target/works-with-heroku-1.0.war root@3.110.135.141:/opt/tomcat/apache-tomcat-9.0.98/webapps/works-with-heroku-1.0.war"

              // Restart Tomcat to deploy new version
              sh "ssh root@3.110.135.141 'sudo systemctl restart tomcat'"
              }
            }
        }
    }
}

