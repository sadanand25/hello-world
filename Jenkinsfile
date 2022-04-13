node {
    stage("SCM Checkout"){
        steps{
            git 'https://github.com/sadanand25/hello-world.git'
        }
    stage("MVN Package"){
      sh ' mvn clean install'
        }

    }
   
}