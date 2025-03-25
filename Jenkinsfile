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

            // Ensure the WAR file exists before copying
            sh "ls -lh target/works-with-heroku-1.0.war"

            // Copy WAR file to Tomcat's webapps directory
            sh "sudo cp target/works-with-heroku-1.0.war /opt/tomcat/apache-tomcat-9.0.98/webapps/works-with-heroku-1.0.war"

            // Restart Tomcat
            sh "sudo systemctl restart tomcat"
              }
            }
        }
    }
}

