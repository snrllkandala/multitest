pipeline {
    agent any
    
    environment {
       muleRuntime = '4.3.0'
       appName_dev = 'multibranch-example-dev'
       appName_prod = 'multibranch-example-prod'
       worker_dev = '1'
       workerType_dev = 'MICRO'
       worker_prod = '1'
       workerType_prod = 'MICRO'
       region ='us-east-1'
       server = 'Anypoint'
       environment_prod = 'Sandbox'
       environment_dev = 'dev'
    }

    stages {
        stage('Build') {
            steps {
            echo "${server}"
                bat 'mvn clean install -DskipTests'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy To Development Server') {
        when {
                branch 'dev' 
            }
            steps {
            
		configFileProvider([configFile(fileId: '11f7d459-6347-40e1-9d92-ad2b428c4b6b', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
               bat "mvn -gs ${MAVEN_GLOBAL_SETTINGS} deploy -DmuleDeploy -Dserver=${server} -Dregion=${region} -Denvironment=${environment_dev} -Dworkers=${worker_dev} -DworkerType=${workerType_dev} -Dapp.runtime=${muleRuntime} -Dapp.name=${appName_dev}"
            }}
        }
        
        stage('Deploy To Prod Server') {
         when {
                branch 'main' 
            }
            steps {
           
		configFileProvider([configFile(fileId: '11f7d459-6347-40e1-9d92-ad2b428c4b6b', variable: 'MAVEN_GLOBAL_SETTINGS')]) 
		{
                bat  "mvn -gs ${MAVEN_GLOBAL_SETTINGS} deploy -DmuleDeploy -Dserver=${server} -Dregion=${region} -Denvironment=${environment_prod} -Dworkers=${worker_prod} -DworkerType=${workerType_prod} -Dapp.runtime=${muleRuntime} -Dapp.name=${appName_prod}"
        }
        }
        }
    }
}