pipeline{
    agent any
    parameters {
    string(
      name: 'Description',
      defaultValue: 'This is default value',
      description: 'Please enter short description'
    )
    choice( name: 'env', choices: ['DEV', 'TEST'] , description: "Choose ENV?" )
    }
    stages{
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
                sh "mvn clean test install -f JUnitHelloWorld/pom.xml"

            }
        }
        stage('Pushing jar to Artifactory')
        {
            steps{
                rtServer (
                    id: 'JUnit-Artifactory',
                    url: 'http://54.160.21.79:8082//artifactory',
                    username: 'admin',
                    password: 'Devops2020',
                    timeout: 300
                )
                rtUpload (
                    serverId: 'JUnit-Artifactory',
                    spec: '''{
                          "files": [
                            {
                              "pattern": "$WORKSPACE/JUnitHelloWorld/target/*.jar",
                              "target": "DemoMavenRepo/$BUILD_ID $JOB_NAME/"
                            }
                          ]
                    }'''
                )
            }
        }
        stage('Create Incident In SNOW')
        {
            steps{
                echo 'Creating Incident ID'
                sh 'echo \"{\'short_description\':\'`date` ${Description}\',\'urgency\':\'3\',\'impact\':\'2\'}\" > mydata.json'
        sh 'curl https://dev51182.service-now.com/api/now/table/incident -X POST -H Accept:application/json -H Content-Type:application/json --data @mydata.json --user admin:gVlCBvfE4aZ1'
                //echo '=========================Response===================' + response
            }
        }
        stage('Deploy')
        {
            steps{
                rtServer (
                    id: 'JUnit-Artifactory',
                    url: 'http://54.160.21.79:8082//artifactory',
                    username: 'admin',
                    password: 'Devops2020',
                    timeout: 300
                )
                rtDownload (
                    serverId: 'JUnit-Artifactory',
                    spec: '''{
                  "files": [
                {
                  "pattern": "DemoMavenRepo/$BUILD_ID $JOB_NAME/*.jar",
                  "target": "$WORKSPACE/${env}/"
            }
          ]
    }'''
)
                }
        }
        
    }
    
}
