pipeline {
  agent{
    docker{
      image 'python:2-alpine'
      image 'qnib/pytest'
      image 'cdrx/pyinstaller-linux:python2'
      args '-p 2400:2400'
    }
  }
  stages{
    stage('Build'){
      steps{
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      }
    }
    stage('Test'){
      steps{
       sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
      post {
          always {
            junit 'test-reports/results.xml'
          }
      }
    }
    stage('Deploy'){
      steps{
        sh 'pyinstaller --onefile sources/add2vals.py'
      }
      post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
      }
    }
  }
}