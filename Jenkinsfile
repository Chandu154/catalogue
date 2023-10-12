pipeline {
    agent { node { label 'AGENT-1' } }
    
    environment{
        //here if we create any variables we will have global access, since it is environment no need pf def
    packageVersion= ''
    }
    stages {
        stage('Get version'){
            steps{
                script{
                def packageJson = readJSON(file: 'package.json')
                 packageVersion = packageJson.version
                echo "version: ${packageVersion}"
            }
            
            }
        }
        stage('Install depdencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Unit test') {
            steps {
                echo "unit testing is done here"
            }
        }
        //sonar-scanner command expect sonar-project.properties should be available
        // stage('Sonar Scan') {
        //     steps {
        //         sh 'ls -ltr'
        //         sh 'sonar-scanner'
        //     }
        // }
        stage('Build') {
            steps {
                sh 'ls -ltr'
                sh 'zip -r catalogue.zip ./* --exclude=.git --exclude=.zip'
            }
        }

        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: '172.31.88.69:8081/',
                    groupId: 'com.roboshop',
                    version: "$packageVersion",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }



        stage('Deploy') {
            steps {
                echo "Deployment"
            }
        }
    }

    post{
        always{
            echo 'cleaning up workspace'
            //deleteDir()
        }
    }
}

// stage('Get version'){
//             steps{
//                 def packageJson = readJSON file: 'package.json'
//             def packageVersion = packageJson.version
//             echo "version: ${packageVersion}"
//             }
//         }