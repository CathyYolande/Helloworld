pipeline {
    agent any
     stages{
      stage('Hello'){
       steps {
         echo "Hello World"
       }
    }
      stage("build"){
        steps{
          sh 'maven clean'
        }
      }
    
    }

}
