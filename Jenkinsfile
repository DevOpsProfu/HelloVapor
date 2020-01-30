pipeline {
  agent {
    node {
      label 'MacAndroid'
    }

  }
  stages {
    stage('Compilacion Prod') {
      steps {
        sh '''withEnv(["ANDROID_HOME=/Users/devops/Library/Android/sdk", "PATH+ANDROID_HOME=/Users/devops/Library/Android/sdk:${ANDROID_HOME}"]) {
            echo \'Descarga del repositorio de Github:\'+"$urlGit"    
            checkout([$class: \'GitSCM\',
            branches: [[name: "$branch"]], 
            doGenerateSubmoduleConfigurations: false,
            extensions: [], 
            submoduleCfg: [], 
            userRemoteConfigs: [[credentialsId: \'devops\', 
            url: "$urlGit"]]]) 
        
            echo \'Inicia compilacion\'
            try{
                if (destino==\'develop\'){
                    sh label: \'\', script: \'\'\'cd source
                        ./gradlew assembleDebug \'\'\'
                } else if (destino==\'qa\'){
                    sh label: \'\', script: \'\'\'cd source
                        ./gradlew assembleQA \'\'\'
                } else if (destino==\'release\'){
                    sh label: \'\', script: \'\'\'cd source
                        ./gradlew assembleRelease -Pandroid.injected.signing.store.file=/Users/devops/Documents/Android/workspace/Test_Release/source/app/profuturo.key -Pandroid.injected.signing.store.password=profuturo -Pandroid.injected.signing.key.alias=profuturo\\\\ movil -Pandroid.injected.signing.key.password=sitioMovil#1 --debug \'\'\'                        
                }
            }catch(Exception e){
                currentBuild.result = \'FAILURE\'
                emailext attachLog: true,  body: \'La compilacion Release del proyecto \'+env.JOB_NAME+\' ha fallado, se anexa log de la construccion \'+env.BUILD_NUMBER, from: \'devops@profuturo.com.mx\', subject: \'Fallo de compilacion Release del proyecto\'+env.JOB_NAME+\' construccion(\'+env.BUILD_NUMBER+ \').\', to: "$ltCorreo" 
                error(\'La compilacion Release del proyecto \'+env.JOB_NAME+\' ha fallado\')
            }
        }'''
        }
      }
    }
  }