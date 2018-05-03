pipeline {
  agent any
  stages {
    stage('installation') {
      steps {
        sh 'python2.7 /var/tmp/Nightswatch/deploy/upgrade_machines_sa.py -p $target_cluster --ininame \'/var/lib/jenkins/nightswatch/5.1/config.ini\''
      }
    }
    stage('fragment_cache') {
      steps {
        build (job:'Fragment_cache',propagate: false,wait: false)
      }
    }
    stage('thumbnails_rb') {
      steps {
        build (job:'test_thumbnails_rb_5.1',propagate: false,wait: false)
      }
    }
    stage('thumbnails_vod') {
      steps {
        build (job:'test_thumbnails_vod_5.1',propagate: false,wait: false)
      }
    }
    stage('at_lru') {
      steps {
        build (job:'test_at_lru_cache_5.1',propagate: false,wait:false)
      }
    }
    stage('copy xmls') {
      steps {
        sh '''scp -p root@$target_cluster:/tmp/Fragment_cache/*.xml .
scp -p root@$target_cluster:/tmp/Thumbnails/*.xml .
scp -p root@$target_cluster:/tmp/LRU_cache/*.xml .'''
      }
    }
    stage('check_cores') {
      steps {
        sh 'ssh root@$target_cluster "python2.7 /var/Nightswatch/deploy/find_cores.py"'
      }
    }
    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 0.1)
      }
    }
  }
  environment {
    target_cluster = '10.65.173.162'
  }
}
