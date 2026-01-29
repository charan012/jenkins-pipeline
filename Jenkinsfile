node('slavenode') {

    def startTime = System.currentTimeMillis()
    def mvnHome = tool 'maven3'

    try {

        stage('Checkout') {
            git branch: 'main',
                credentialsId: 'gitcreds',
                url: 'https://github.com/charan012/jenkins-pipeline.git'
        }

        stage('Build') {
            sh "${mvnHome}/bin/mvn clean compile" 
        }

        stage('Test') {
            sh "${mvnHome}/bin/mvn test"
        }

        stage('Package') {
            sh "${mvnHome}/bin/mvn package"
        }

        stage('Archive') {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }

        def endTime = System.currentTimeMillis()
        def duration = (endTime - startTime) / 1000

        echo "Scripted pipeline executed successfully in ${duration} seconds"

        emailext(
            subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """
Hello SRAVAN,

Your scripted Jenkins pipeline completed successfully.

 Job Name   : ${env.JOB_NAME}
 Build No   : ${env.BUILD_NUMBER}
 Duration   : ${duration} seconds
 Status     : SUCCESS

 Build URL:
${env.BUILD_URL}

Regards,
Jenkins CI
""",
            to: "charankantamsetty@gmail.com"
        )

    } catch (err) {

        def endTime = System.currentTimeMillis()
        def duration = (endTime - startTime) / 1000

        echo "Scripted pipeline failed after ${duration} seconds"

        emailext(
            subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """
Hello Charan,

Your scripted Jenkins pipeline has FAILED.

Job Name   : ${env.JOB_NAME}
Build No   : ${env.BUILD_NUMBER}
Duration   : ${duration} seconds
Status     : FAILURE

Build URL:
${env.BUILD_URL}

Please check the console logs for details.

Regards,
Jenkins CI
""",
            to: "charankantamsetty@gmail.com"
        )

        throw err
    }
}
