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
            def warFile = "target/works-with-heroku-1.0.war"  // WAR file path
            def tomcatPath = "/opt/tomcat/apache-tomcat-9.0.98/webapps"  // Tomcat webapps directory

             // Stop Tomcat manually
                    sh "sh /opt/tomcat/apache-tomcat-9.0.98/bin/shutdown.sh || true"

                    // Deploy WAR file
                    sh "sudo cp target/works-with-heroku-1.0.war /opt/tomcat/apache-tomcat-9.0.98/webapps/works-with-heroku-1.0.war"

                    // Start Tomcat manually
                    sh "sh /opt/tomcat/apache-tomcat-9.0.98/bin/startup.sh"

              }
            }
        }
    }
}

