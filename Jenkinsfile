/*
This is a example of how to implement the CxSAST scan using the jenkins pipelines scripting.
*/

pipeline { 
    agent any
    environment {
        /*
            Those variables were configured as a Text secret under the Jenkins Credentials, to use an example that the CxSAST step can accept plaintext for the password, user, and URL parameters.
            
            Please refer to the Jenkins documentation below to see other options to create secrets or credentials for the Jenkins pipelines.
            https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#handling-credentials
        */
        SAST_USER_PASSWORD = credentials('SAST_USER_PASSWORD')
        SAST_USER_NAME = credentials('SAST_USER_NAME')
        SAST_URL = credentials('SAST_URL')
    }
    stages {
        stage('Git') {
            steps {
                git branch: 'jenkins_pipes', url: 'https://github.com/TaynaCT/Testing_Goat/'
            }
        }
        stage("Checkmarx SAST Analysis") {
            steps {
                script {
                    step([
                            $class: 'CxScanBuilder', 
                            comment: '', 
                            configAsCode: false, 
                            credentialsId: 'RemoteSAST', 
                            customFields: '', 
                            excludeFolders: '', 
                            exclusionsSetting: 'global',
                            failBuildOnNewResults: false,
                            failBuildOnNewSeverity: 'HIGH',
                            filterPattern: '''!**/_cvs/**/*, !**/.svn/**/*, !**/.hg/**/*, !**/.git/**/*, !**/.bzr/**/*,
                                !**/.gitgnore/**/*, !**/.gradle/**/*, !**/.checkstyle/**/*, !**/.classpath/**/*, !**/bin/**/*,
                                !**/obj/**/*, !**/backup/**/*, !**/.idea/**/*, !**/*.DS_Store, !**/*.ipr, !**/*.iws,
                                !**/*.bak, !**/*.tmp, !**/*.aac, !**/*.aif, !**/*.iff, !**/*.m3u, !**/*.mid, !**/*.mp3,
                                !**/*.mpa, !**/*.ra, !**/*.wav, !**/*.wma, !**/*.3g2, !**/*.3gp, !**/*.asf, !**/*.asx,
                                !**/*.avi, !**/*.flv, !**/*.mov, !**/*.mp4, !**/*.mpg, !**/*.rm, !**/*.swf, !**/*.vob,
                                !**/*.wmv, !**/*.bmp, !**/*.gif, !**/*.jpg, !**/*.png, !**/*.psd, !**/*.tif, !**/*.swf,
                                !**/*.jar, !**/*.zip, !**/*.rar, !**/*.exe, !**/*.dll, !**/*.pdb, !**/*.7z, !**/*.gz,
                                !**/*.tar.gz, !**/*.tar, !**/*.gz, !**/*.ahtm, !**/*.ahtml, !**/*.fhtml, !**/*.hdm,
                                !**/*.hdml, !**/*.hsql, !**/*.ht, !**/*.hta, !**/*.htc, !**/*.htd, !**/*.war, !**/*.ear,
                                !**/*.htmls, !**/*.ihtml, !**/*.mht, !**/*.mhtm, !**/*.mhtml, !**/*.ssi, !**/*.stm,
                                !**/*.bin,!**/*.lock,!**/*.svg,!**/*.obj,
                                !**/*.stml, !**/*.ttml, !**/*.txn, !**/*.xhtm, !**/*.xhtml, !**/*.class, !**/*.iml, !Checkmarx/Reports/*.*,
                                !OSADependencies.json, !**/node_modules/**/*''',
                            fullScanCycle: 10,
                            groupId: '1',
                            password: SAST_USER_PASSWORD,
                            preset: '36',
                            projectName: 'jenkins_pipes',
                            sastEnabled: true,
                            serverUrl: SAST_URL,
                            sourceEncoding: '1',
                            useOwnServerCredentials: true,
                            username: SAST_USER_NAME, 
                            vulnerabilityThresholdResult: 'FAILURE',
                            waitForResultsEnabled: true
                    ])
                }
            }
        }
    }
}
