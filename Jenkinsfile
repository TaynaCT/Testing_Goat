/*
Documentation reference:
https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#handling-credentials
*/

pipeline { 
    agent any
    environment {
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
                            $class                       : 'CxScanBuilder',
                            exclusionsSetting            : 'global',
                            vulnerabilityThresholdEnabled: true,
                            failBuildOnNewResults        : true,
                            failBuildOnNewSeverity       : 'HIGH',
                            filterPattern                :
                                    '''!**/_cvs/**/*, !**/.svn/**/*,   !**/.hg/**/*,   !**/.git/**/*,  !**/.bzr/**/*, !**/bin/**/*,
                            !**/obj/**/*,  !**/backup/**/*, !**/.idea/**/*, !**/*.DS_Store, !**/*.ipr,     !**/*.iws,
                            !**/*.bak,     !**/*.tmp,       !**/*.aac,      !**/*.aif,      !**/*.iff,     !**/*.m3u, !**/*.mid, !**/*.mp3,
                            !**/*.mpa,     !**/*.ra,        !**/*.wav,      !**/*.wma,      !**/*.3g2,     !**/*.3gp, !**/*.asf, !**/*.asx,
                            !**/*.avi,     !**/*.flv,       !**/*.mov,      !**/*.mp4,      !**/*.mpg,     !**/*.rm,  !**/*.swf, !**/*.vob,
                            !**/*.wmv,     !**/*.bmp,       !**/*.gif,      !**/*.jpg,      !**/*.png,     !**/*.psd, !**/*.tif, !**/*.swf,
                            !**/*.jar,     !**/*.zip,       !**/*.rar,      !**/*.exe,      !**/*.dll,     !**/*.pdb, !**/*.7z,  !**/*.gz,
                            !**/*.tar.gz,  !**/*.tar,       !**/*.gz,       !**/*.ahtm,     !**/*.ahtml,   !**/*.fhtml, !**/*.hdm,
                            !**/*.hdml,    !**/*.hsql,      !**/*.ht,       !**/*.hta,      !**/*.htc,     !**/*.htd, !**/*.war, !**/*.ear,
                            !**/*.htmls,   !**/*.ihtml,     !**/*.mht,      !**/*.mhtm,     !**/*.mhtml,   !**/*.ssi, !**/*.stm,
                            !**/*.stml,    !**/*.ttml,      !**/*.txn,      !**/*.xhtm,     !**/*.xhtml,   !**/*.class, !**/*.iml, !Checkmarx/Reports/*.*''',
                            fullScanCycle                : 10,
                            osaArchiveIncludePatterns    : '*.zip, *.war, *.ear, *.tgz',
                            osaEnabled                   : true,
                            osaInstallBeforeScan         : false,
                            password                     : SAST_USER_PASSWORD,
                            username                     : SAST_USER_NAME,
                            serverUrl                    : SAST_URL,
                            preset                       : '36',
                            projectName                  : 'jenkins_pipes',
                            sastEnabled                  : true,                         
                            sourceEncoding               : '1',
                            useOwnServerCredentials      : true,                            
                            vulnerabilityThresholdResult : 'FAILURE',
                            waitForResultsEnabled        : true,
                    ])
                }
            }
        }
    }
}
