pipeline {
  agent any
  environment {
    
	DEPLOY_CREDS = credentials('deploy_anypoint_fst_non_prod')
	FST_DEV_CREDS = credentials('mulesoft_client_fst')
	MULE_KEY_CREDS = credentials('its_mule_key_dev_prd')
	
    MULE_VERSION = '4.4.0'
    //Worker Size or Worker Type = Micro or Small
    WORKER_TYPE = "Small"
    //Workers values 1 to 6
    WORKERS = "2"
    BUS_GRP="FST"
	
  }
  stages {
    stage('Build') {
      steps {
        bat 'mvn -B -U -e -V clean -DskipTests package'
      }
    }
/*    stage('Test') {
      steps {
        bat 'mvn test'
      }
    }
*/    stage('Deploy Development') {
      environment {
        ENV_P = "${ENV}"
        APP_NAME_P = "uchicago-${APP_NAME}-${ENV}"
      }
      steps {
        echo "Application $APP_NAME_P is deploying to environment $ENV_P"
        bat 'mvn -U -V -e -B -DskipTests clean package deploy:deploy mule:deploy -Dmule.version="%MULE_VERSION%" -Dch.app="%APP_NAME_P%" -Dch.env="%ENV_P%" -Dch.group="%BUS_GRP%" -Dch.workerType="%WORKER_TYPE%" -Dch.workers="%WORKERS%" -DdeploymentTimeout="2000000" -Dch.ca_id="%DEPLOY_CREDS_USR%" -Dch.ca_secret="%DEPLOY_CREDS_PSW%" -Dch.id="%FST_DEV_CREDS_USR%" -Dch.secret="%FST_DEV_CREDS_PSW%" -Dch.key="%MULE_KEY_CREDS_USR%" -Danalytics="true"'
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}
