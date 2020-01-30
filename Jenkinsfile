pipeline{
agent any
    stages{
		stage('Compilación Prod'){
			steps{
				script{
					def inputStep = input(
						message: "¿Otorga el Vobo para salir a producción?",
						ok: "Si",
						submitterParameter: "APPROVER"
					) 
				}
			}
		}
	}
}
