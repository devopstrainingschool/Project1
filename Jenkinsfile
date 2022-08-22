pipeline {
agent any
  tools {
    maven "maven-yaya"
  }
  stages {
    
    stage ('Maven Clean'){
      steps{
        sh 'mvn clean'
      }
    }
    stage ('Maven package') { 
      steps{
        sh 'mvn package'
      }
    }
    stage ('Sonarqube analysis and tesing'){
      steps{
        script{
          withSonarQubeEnv('sonarserver'){
            sh 'mvn clean package sonar:sonar'
          }
        }
      }
    }    
   stage ("Quality Gate") {
      steps {
        script {
           timeout(time: 1, unit: 'HOURS') { 
        def qg = waitForQualityGate() 
        if (qg.status != 'OK') {
             error "Pipeline aborted due to quality gate failure: ${qg.status}"
        }
           }
        }
      }
    }
    stage ('Docker build and push'){
   
           steps{
             withDockerRegistry([ credentialsId: "Docker_creds", url: "https://index.docker.io/v1/" ]){
               sh 'docker build -t devopstrainingschool/knote-jenkins:$BUILD_NUMBER . -f Dockerfile'
               sh 'docker push devopstrainingschool/knote-jenkins:$BUILD_NUMBER'
             }
           }
    }
    
    stage('Push the changed deployment file to Git'){
            steps {
                script{
                  catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh """
                    sh "git clone https://github.com/devtopstrainingschool/k8s-all-p1.git"
                    sh "sed -i 's+devopstrainingschool/knote*+devopstrainingschool/knote-jenkins:$BUILD_NUMBER+g' k8s-all-p1/webapp.yaml"
                    git config --global user.name "yannick"
                    git config --global user.email "yannickeboo@gmail.com"
                    dir("k8s-all-p1") {
                    git add .
                    git commit -m "hsh"
                    git push 
                    }
                    """
                    
                   
                    }
                }
            }
        }
    }

    
    
    
    
    
  }
}

