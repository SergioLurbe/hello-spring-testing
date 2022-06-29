pipeline {
    agent any

    stages {
        /*
        stage('Test') {
            steps {
                sh './gradlew clean test check pitest'
            }
            post {
                always {
                    junit 'build/test-results/test/*.xml'
                    jacoco execPattern: 'build/jacoco/*.exec'
                    recordIssues(
                        tools: [
                            pmdParser(pattern: 'build/reports/pmd/*.xml')
                        ]
                    )
                    recordIssues(tools: [pit(pattern: 'build/reports/pitest/*.xml')])
                }
            }
        }
        */
        stage('Build') {
            steps {
                // Run Gradle Wrapper
                sh "./gradlew assemble"
            }
            post {
                // If Gradle was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    archiveArtifacts 'build/libs/*.jar'
                }
            }
        }
        stage('Deploy') {
            steps {
                   echo 'Deploying...'
            }
        }

        stage('Publish') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockergit', passwordVariable: 'TOKEN', usernameVariable: 'USERNAME')]) {
                    sh 'gradle publish'
                }
            }
        }


    }
}








/*
pipeline {
    agent any
    options {
        ansiColor('xterm')
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '5'))
    }
    stages {
        stage('Build') {
            steps {
               sh "./gradlew test assemble check"

            }

          post {
                success {
                    junit 'build/test-results/test/*.xml'
                    archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint:true, followSymlinks:false
                    jacoco()
                    recordIssues enabledForFailure: true, tool: pmdParser(pattern: 'build/reports/pmd/*.xml')

                }
            }
        }

        stage('Publish'){
            steps {
                sshagent(['github-ssh']) {
                sh 'git tag BUILD-1.0.${BUILD_NUMBER}'
                sh 'git push --tags'
                }
            }
        }
    }
}

*/