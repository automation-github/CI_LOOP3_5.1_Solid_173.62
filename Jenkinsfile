pipeline {
  agent any
  stages {
    stage('installation') {      
        stage('installation') {
          steps {
            sh 'python2.7 /var/tmp/Nightswatch/deploy/upgrade_machines_sa.py -p $target_cluster --ininame \'/var/lib/jenkins/nightswatch/5.1/config.ini\''
          }
        }
        stage('fragment_cache_rb') {
          steps {
            build 'test_fragment_cache_rb_5.1'
          }
        }
        stage('Fragment_cache_rb_info') {
          steps {
            build 'test_fragment_cache_fragmentinfo_5.1'
          }
        }
        stage('thumbnails_rb') {
          steps {
            build 'test_thumbnails_rb_5.1'
          }
        }
        stage('thumbnails_vod') {
          steps {
            build 'test_thumbnails_vod_5.1'
          }
        }
        stage('at_lru') {
          steps {
            build 'test_at_lru_cache_5.1'
          }
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
