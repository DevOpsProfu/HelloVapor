pipeline {
agent any
    stages{
        stage('Programacion de liberación'){
            steps{
                script{
                    def inputStep = input(
                        message: "¿Ingrese los valores requeridos?",
                        ok: "Si",
                        parameters:[    
                            [$class: 'TextParameterDefinition', defaultValue: '####', description: 'Numero de liberación', name: 'liberacionNo'],
                            [$class: 'TextParameterDefinition', defaultValue: 'Proyecto', description: 'Tipo de liberación', name: 'proyectoTipo'],
                            [$class: 'TextParameterDefinition', defaultValue: "26/11/2019 08:00 P.M.", description: 'Fecha de liberacion', name: 'liberacionFecha'],
                            //[$class: 'ChoiceParameterValue', name: 'choiceParam', value: "1\n\2\n3\n"]
                            //[$class: 'DateParameterDefinition', name:"FechaLiberacion",dateFormat: 'ddMMYY', defaultValue: 'LocalDate.now()',description: 'Fecha de liberación solicitada']
                         ]
                    )
                }
            }
        }
	}

	stages{
        stage('Aprobacion Usuario'){
			steps{
				script{
					try{
						def inputStep = input(
							message: "¿Otorga el Vobo para salir a producción?",
							ok: "Si",
							submitterParameter: "APPROVER"
						) 
					}
					catch(err){
						echo 'No se promovera a release'
						currentBuild.result = 'ABORTED'
						githubPRClosePublisher statusVerifier: allowRunOnStatus('ABORTED')
						error 'Se aborto pase a producción'
					}
				}
			}
        }

        stage('Aprobacion Control de Cambios'){
			steps{
				script{
					try{
						def inputStep = input(
							message: "¿Otorga el Vobo para salir a producción?",
							ok: "Si",
							submitterParameter: "APPROVER"
							)
						}
					catch(err){
						echo 'No se promovera a Producción'
						currentBuild.result = 'ABORTED'
						githubPRClosePublisher statusVerifier: allowRunOnStatus('ABORTED')
						error 'Se aborto pase a producción'
					}
				}
			}
        }    
	}
	
	stages{
        stage('Notificacion GitHub'){  
			steps{
				script{
					echo 'Ingresa comentarios y aprueba pullrequest'    
					currentBuild.result = 'SUCCESS'
					//githubPRComment comment: githubPRMessage('Se aprueba pull request con el build ${BUILD_NUMBER}'), statusVerifier: allowRunOnStatus('SUCCESS')
					//githubPRStatusPublisher buildMessage: message(failureMsg: githubPRMessage('Can\'t set status; build failed.'), successMsg: githubPRMessage('Can\'t set status; build succeeded.')), statusMsg: githubPRMessage('Se aprueba pull request con el build ${BUILD_NUMBER}'), statusVerifier: allowRunOnStatus('SUCCESS'), unstableAs: 'FAILURE'
				}
			}
		}
    }
}
