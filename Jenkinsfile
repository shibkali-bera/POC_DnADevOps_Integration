node {

    def mvnHome = tool 'Maven3'
    
        stage('Poll Config file changes from GitHub')
        {
            steps{
                echo 'Fetch Config file changes from GitHub'
                git branch: 'dev', credentialsId: 'df8d2f9a-b1f5-43ae-a31c-fa4e163c1bf4', url: 'https://github.com/shibkali-bera/POC_DnADevOps_Integration.git'
            }
        }
	stage('build')
        {
            steps{
                echo 'Fetch Config file changes from GitHub'
                sh "${mvnHome}/bin/mvn clean test install -f JUnitHelloWorld/pom.xml"

            }
        }

}
