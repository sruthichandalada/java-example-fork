pipeline {
    agent { label 'build' }

    stages {
        stage('Git Checkout Stage') {
            steps {
                git branch: 'main', url: 'https://github.com/sruthichandalada/java-example-fork.git'
            }
        }

        stage('Build Stage') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis Stage') {
            steps {
                withSonarQubeEnv('sonarqube-server') { 
                    sh "mvn clean verify sonar:sonar"
                }
            }
        }

        stage('Deploy to Tomcat') {
            agent { label 'tomcat' }  // Running on the Tomcat agent
            steps {
                script {
                    def warFile = "target/java-web-app.war"  // Adjust based on your actual WAR file name
                    def tomcatUser = "tomcat"  // Update with actual username
                    def tomcatHost = "http://13.235.254.146"  // Update with Tomcat server IP or hostname
                    def tomcatPath = "/opt/tomcat/apache-tomcat-9.0.98/webapps"  // Adjust if needed


                    // Transfer the WAR file to the Tomcat server
                    sh "scp works-with-heroku-1.0.war build@13.235.254.146:/opt/tomcat/apache-tomcat-9.0.98/webapps/java-web-app.war"

                    // Restart Tomcat server
                    sh "ssh build@13.235.254.146 'systemctl restart tomcat'"
                }
            }
        }
    }
}
