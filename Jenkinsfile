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
           stage('Deploy to Remote Tomcat') {
           agent {label 'tomcat'}
            steps {
                script {
                    def warFile = "target/works-with-heroku-1.0.war"
                    def tomcatUser = "root"  // Use Jenkins user if configured
                    def tomcatHost = "13.235.254.146"   // Remote agent hostname (or IP)
                    def tomcatPath = "/opt/tomcat/apache-tomcat-9.0.98/webapps"

                    // Copy WAR file to remote Tomcat server
                    sh "sudo scp target/works-with-heroku-1.0.war root@13.235.254.146:/opt/tomcat/apache-tomcat-9.0.98/webapps/works-with-heroku-1.0.war"

                    // Restart Tomcat on remote server
                    sh "ssh root@13.235.254.146 '/opt/tomcat/apache-tomcat-9.0.98/bin/shutdown.sh || true'"
                    sh "ssh root@13.235.254.146 '/opt/tomcat/apache-tomcat-9.0.98/bin/startup.sh'"

              }
            }
        }
    }
}

