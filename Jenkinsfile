pipeline {
    agent {
        node {
            label 'BUILD' // use build label for the 4 runners
        }
    }
    triggers {
        cron('0 * * * 1-5')
    }
    options { timestamps () }
    stages {
        stage('build-test') {
            agent { label 'BUILD' }
            when {
                not {
                    expression { return currentBuild.rawBuild.getCause(hudson.triggers.TimerTrigger$TimerTriggerCause) }
                }
            }
            steps {
                sh 'echo arjun > usefulfile.txt'
            }
            post {
                always {
                    archiveArtifacts(artifacts: 'usefulfile.txt', fingerprint: true)
                }
            }
        }
        stage('Tests') {
            when {
                not {
                    expression { return currentBuild.rawBuild.getCause(hudson.triggers.TimerTrigger$TimerTriggerCause) }
                }
            }
            parallel {
                stage('test1'){
                    agent { label 'BUILD' }
                    steps {
                        sh 'echo  test1'
                        sh 'echo ${JOB_NAME}'
                        sh 'echo ${BUILD_NUMBER}'

                        copyArtifacts filter: '*.txt', fingerprintArtifacts: true, projectName: '${JOB_NAME}', selector: specific('${BUILD_NUMBER}')

                        //sh 'echo hi > test.xml'
                        sh 'cat  usefulfile.txt'
                    }
                    post{
                        always {
                            sh 'echo Publishing XML'
                            step([$class: 'JUnitResultArchiver', checksName: '', testResults: 'test.xml'])
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: '*.html', reportName: 'HTML Report', reportTitles: ''])
                        }

                    }
                }
                stage('test2'){
                    agent { label 'BUILD' }
                    steps {
                        sh 'echo  test2'
                        copyArtifacts(projectName: '${JOB_NAME}',
                                filter: '*.txt',
                                fingerprintArtifacts: true,
                                selector: [$class: 'SpecificBuildSelector', buildNumber: '${BUILD_NUMBER}'])
                        sh 'cat usefulfile.txt'
                    }
                    post{
                        always {
                            sh 'echo Publishing XML'
                            step([$class: 'JUnitResultArchiver', checksName: '', testResults: 'test2.xml'])
                            //publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: '*.html', reportName: 'HTML Report', reportTitles: ''])
                        }

                    }
                }
                stage('test3'){
                    agent { label 'BUILD' }
                    steps {
                        sh 'echo  test3'
                        copyArtifacts(projectName: '${JOB_NAME}',
                                filter: '*.txt',
                                fingerprintArtifacts: true,
                                selector: [$class: 'SpecificBuildSelector', buildNumber: '${BUILD_NUMBER}'])
                        sh 'cat usefulfile.txt'
                    }
                    post{
                        always {
                            sh 'echo Publishing XML'
                            step([$class: 'JUnitResultArchiver', checksName: '', testResults: 'test2.xml'])
                            //publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: '*.html', reportName: 'HTML Report', reportTitles: ''])
                        }

                    }
                }
                stage('test4'){
                    agent { label 'BUILD' }
                    steps {
                        sh 'echo  test4'
                        copyArtifacts(projectName: '${JOB_NAME}',
                                filter: '*.txt',
                                fingerprintArtifacts: true,
                                selector: [$class: 'SpecificBuildSelector', buildNumber: '${BUILD_NUMBER}'])

                        sh 'cat usefulfile.txt'

                    }
                    post{
                        always {
                            sh 'echo Publishing XML'
                            step([$class: 'JUnitResultArchiver', checksName: '', testResults: 'test2.xml'])
                            //publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: '*.html', reportName: 'HTML Report', reportTitles: ''])
                        }

                    }
                }
            }
        }
        stage('build-app'){
            when {
                anyOf { branch 'develop';branch 'main';branch 'release' }
                not {
                    expression { return currentBuild.rawBuild.getCause(hudson.triggers.TimerTrigger$TimerTriggerCause) }
                }
            }
            parallel {
                stage('Build-1'){
                    agent { label 'BUILD' }
                    steps {
                        sh 'echo building app > build.txt'
                    }
                    post {
                        always {
                            archiveArtifacts artifacts: 'build.txt', fingerprint: true
                        }
                    }
                }


                stage('Build-2'){
                    agent { label 'BUILD' }
                    steps {
                        sh 'echo building app > build.txt'
                    }
                    post {
                        always {
                            archiveArtifacts artifacts: 'build.txt', fingerprint: true
                        }
                    }

                }
                stage('Build-3'){
                    agent { label 'BUILD' }
                    steps {
                        sh 'echo building app > build.txt'
                    }
                    post {
                        always {
                            archiveArtifacts artifacts: 'build.txt', fingerprint: true
                        }
                    }

                }
                stage('Build-4'){
                    agent { label 'BUILD' }
                    steps {
                        sh 'echo building app > build.txt'
                    }
                    post {
                        always {
                            archiveArtifacts artifacts: 'build.txt', fingerprint: true
                        }
                    }

                }
            }
        }
        stage('Distribute-app'){
            when {
                anyOf { branch 'develop';branch 'main';branch 'release' }
                not {
                    expression { return currentBuild.rawBuild.getCause(hudson.triggers.TimerTrigger$TimerTriggerCause) }
                }
            }
            parallel {
                stage('Distribute-1'){
                    steps{ sh'echo Distribute-1'}
                    // TO-DO: copy artifcats from build 1

                }
                stage('Distribute-2'){
                    steps{ sh'echo Distribute-1'}
                    // TO-DO: copy artifcats from build 2

                }
                stage('Distribute-3'){
                    steps{ sh'echo Distribute-1'}
                    // TO-DO: copy artifcats from build 3

                }
                stage('Distribute-4'){
                    steps{ sh'echo Distribute-1'}
                    // TO-DO: copy artifcats from build 4

                }
            }
        }
        stage('weekly-build'){
            when {
                branch 'develop'
                expression { return currentBuild.rawBuild.getCause(hudson.triggers.TimerTrigger$TimerTriggerCause) }
            }
            steps{
                sh'echo weekly-build'
            }
        }
        stage('Weekly-distribute'){
            when {
                branch 'develop'
            }
            steps{
                sh 'echo weekly distribute'
                // TO-DO: copy artifacts from weekly-build
            }
            post {
                always {
                    archiveArtifacts artifacts: 'usefulfile.txt', fingerprint: true
                }
            }
        }

    }
    post {
        always{
            script{
                if(env.BRANCH_NAME == 'main'){
                    // TO-DO: publish release notes an send to slack
                }
            }
        }
        failure {
            emailext subject: '$DEFAULT_SUBJECT',
                    body: '$DEFAULT_CONTENT',
                    recipientProviders: [
                            [$class: 'DevelopersRecipientProvider'],
                    ],
                    replyTo: '$DEFAULT_REPLYTO',
                    to: '$DEFAULT_RECIPIENTS'
        }
        success {
            script{
                if(env.BRANCH_NAME == 'release'){
                    slackSend (color: '#FFFF00', message: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
                }
            }
        }
    }
}