pipeline { 
  environment {
    registry = "registry.hub.docker.com"
    image = "sysdigdan/hellonode"
    status = ""
  }
  
  agent any  
  
  stages {
    
//     stage('Building Image') {
//       steps {
//         script {
//           dockerImage = docker.build("${image}")
//         }
//       }
//     }

//     stage('Push Image') {
//       steps {
//         script {
//           docker.withRegistry("https://${registry}", 'docker-hub-credentials') {
//               dockerImage.push("${env.BUILD_NUMBER}")
//               dockerImage.push("latest")
//           }
//           sh "echo ${registry}/${image}:${env.BUILD_NUMBER} > sysdig_secure_images"
//         }
//       }
//     }

//     stage ('Windows Image Scan') {
//       steps {
//         echo 'Scanning Windows Image...'
//         sh '''
//         docker run --rm -v ${WORKSPACE}:/outdir quay.io/sysdig/scanning-inspector-linux:latest -t pull -min_severity high -min_days_fix 7 -i mcr.microsoft.com/windows/nanoserver:10.0.17763.1518 -f json -o ./outdir/scanResults.json
//         '''

//         echo 'Checking Scan Results...'
//         sh '''cat scanResults.json | jq .scanPassed'''
        
//         script {
//             def status = sh(script: "cat scanResults.json | jq .scanPassed", returnStdout: true).trim()
//             if (status == 'true') {
//                 echo '0'
//                 sh 'exit 0'
//             } else if (status == 'false') {
//                 echo '1'
//                 sh "exit 1"
//             }
//         }
//       }
//     }
    
    stage('Sysdig Image Scan') {
      steps {
        script {
          // Scan from local image recently built
          sysdig engineCredentialsId: 'sysdig-secure-api-credentials', name: 'sysdig_secure_images', inlineScanning: true bailOnFail: false
          // Scan from repository image recently pushed
          // sysdig engineCredentialsId: 'sysdig-secure-api-credentials', name: 'sysdig_secure_images', inlineScanning: false bailOnFail: false
          // Stop pipeline based on Sysdig Analysis
          // sysdig engineCredentialsId: 'sysdig-secure-api-credentials', name: 'sysdig_secure_images', inlineScanning: true, bailOnFail: true
          // Pull Image to scan and Stop pipeline based on Sysdig Analysis
          sysdig engineCredentialsId: 'sysdig-secure-api-credentials', name: 'sysdig_secure_images', inlineScanning: false, bailOnFail: true
        }
      }
    }
    
  }
}
