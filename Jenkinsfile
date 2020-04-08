pipeline {
    agent any    		 						//############# will work on any plateform
    triggers {           						//############# for triggering the pipeline
        pollSCM 'H/10 * * * *' 					//############# Will run in every 10 minutes
    }
    customWorkspace "/var/lib/jenkins/java-project-${date}"       //###### date will of server. can be changed as per requirement.
    tools{														  //### to run build, tools are required.
        maven 'MAVEN_HOME'
        jdk 'JAVA_HOME'
    }
        environment {                                                      //#########code will be fetched from this repository and branch
        git_repo = "https://gitlab.com/kagarwal0205/sampleproject.git"
        git_branch = "master"
        build_data_path = "/var/lib/jenkins/build-data-${date}"           //########## code will be stored under this directory with date. 
        }
//##########################
    stages {
        stage('Stage 1: Pre build setup'){
            steps {
                sh '''
                echo "PATH = ${PATH}"
                echo "MAVEN_HOME = /opt/apache-maven-3.6.1"
                echo "JAVA_HOME = /opt/jdk1.8.0_211-amd64"
                '''
            }
        }
        stage('Stage 2: Git Checkout') { 
            steps {
                step([$class: 'WsCleanup'])                             //##########to clean up work space after job is run.
                script {
                    dir("${WORKSPACE}") {
                        git branch: "${git_branch}", url: "${git_repo}" //######## variables are picked from Environment column
                         }
                        }
                     }
                }
        stage('Stage 3: Build' ) {
            steps {
                 sh '''
                 cd "${WORKSPACE}/"
                 mvn clean install              //mvn needs POM.xml to run this build.
                 '''
              }
        }
        stage('Stage 4: copy artifact') {
            steps{
                sh '''
                cd "${WORKSPACE}/target"
                cp *.jar "${build_data_path}"    //########## copying jar files to build path. 
                '''
            }
        }
    }
}
