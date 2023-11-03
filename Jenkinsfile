pipeline{

agent any;

stages {
	
	stage('git-checkout') {
	steps {
        echo "git-checkout stage is running" 
        git branch: 'main', credentialsId: 'devopsbaergmailcom', url: 'https://gitlab.com/hbaer1/devops.git'
    
	}
	}
	
  stage('maven-build') {
  when { expression { params.ACTION == 'build' || params.ACTION == 'deploy' } }
    steps {
		echo "Maven Build stage is running" 
	  sh 'mvn -f java-tomcat-sample/pom.xml clean deploy'
    }
  }

  stage('deploy-dev') {
  when { expression { params.ACTION == 'deploy' && params.ENV == 'dev' } }
    steps {
		echo "deploy-dev stage is running"
              sh 'mvn -f java-tomcat-sample/pom.xml clean deploy'
		sh '''
		mv $WORKSPACE/java-tomcat-sample/target/*.war $WORKSPACE/java-tomcat-sample/target/dev.war
		scp $WORKSPACE/java-tomcat-sample/target/*.war tomcat@18.170.227.98:/opt/tomcat/webapps/
		'''
    }
  }

  stage('deploy-qa') {
  when { expression { params.ACTION == 'deploy' && params.ENV == 'qa' } }
    steps {
		echo "deploy-qa stage is running"
              sh 'mvn -f java-tomcat-sample/pom.xml clean deploy'
		sh '''
		mv $WORKSPACE/java-tomcat-sample/target/*.war $WORKSPACE/java-tomcat-sample/target/qa.war
		scp $WORKSPACE/java-tomcat-sample/target/*.war tomcat@18.170.227.98:/opt/tomcat/webapps/
		'''
    }
  }
  
  }
  
  post {
		always {
			cleanWs() 
		}
	}
}
