pipeline {
  agent any
  stages {
    stage('installation') {
      steps {
        sh 'python2.7 /var/tmp/Nightswatch/deploy/upgrade_machines_sa.py -p $target_cluster --ininame \'/var/lib/jenkins/nightswatch/5.1/config.ini\''
      }
      steps {
        build '../Fragment_cache/master'
      }
    }       
  
  }
}
