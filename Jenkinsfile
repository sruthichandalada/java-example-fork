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
             agent { label 'tomcat' }  // Runs on the Tomcat-labeled agent
             steps {
               script {
                 def warFile = "target/works-with-heroku-1.0.war"  // Adjust based on actual .war file
                 def tomcatUser = "root"  // Linux user on the Tomcat server
                 def tomcatHost = "13.235.254.146"  // Tomcat server IP or hostname
                 def tomcatPath = "/opt/tomcat/apache-tomcat-9.0.98/webapps"  // Path to Tomcat's webapps directory

              // Copy WAR file to Tomcat server
              sh "scp works-with-heroku-1.0.war root@13.235.254.146:/opt/tomcat/apache-tomcat-9.0.98/webapps/works-with-heroku-1.0.war"

              // Restart Tomcat to deploy new version
              sh "ssh root@13.235.254.146 'systemctl restart tomcat'"
              }
            }
        }
    }
}

