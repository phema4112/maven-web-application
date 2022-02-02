node {
    def MAVENHOME = tool name: "maven 3.8.2"
	echo "GitHub Branch Name ${env.BRANCH_NAME}"
	echo "GitHub Job Number ${env.BUILD_NUMBER}"
	echo "GitHub Node Name ${env.NODE_NAME}"
	echo "jenkins home ${env.JENKINS_HOME}"
	echo "Jenkins URL ${env.JENKINS_URL}"
	echo "Job Name ${env.JOB_NAME}"
	
    stage('checkoutcode'){
        git branch: 'development', credentialsId: '2344cfae-da15-4e62-9c6a-60afce06b6a1', url: 'https://github.com/phema4112/maven-web-application'
    }
    stage('build'){
        sh "${MAVENHOME}/bin/mvn clean package"
    }
    stage('execute sonarqube report'){
        sh "${MAVENHOME}/bin/mvn clean sonar:sonar"
    }
    stage('upload artifact into nexus'){
        sh "${MAVENHOME}/bin/mvn clean deploy"
    }
    stage('deploy app into tomcat'){
        sshagent(['c8137d88-68cc-49fa-8207-5b3eb1a200c4']) {
    sh "scp -o strictHostkeychecking=no target/maven-web-application.war ec2-user@3.109.154.196:/opt/apache-tomcat-9.0.58/webapps/"
}
    }
    stage('sending an email notification'){
        emailext body: '''Build is Over ..!!
        Regards,
        mihuntechnolgies,
        9787721334.''', subject: 'Build Over...!!', to: 'palukuruhemalatha4112@gmail.com'
    }
}
