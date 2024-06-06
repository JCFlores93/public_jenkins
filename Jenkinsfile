
pipeline {
    agent any
    parameters {
        string(name: 'cron_value', defaultValue: '50 19 * * *', description: 'Valor cron para la planificación')
    }
    triggers {
        cron '*/1 * * * *'
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building..."'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Testing..."'
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'
            }
        }
        stage('triggerSeedJob') {
            steps {
                script {
                    try {
                        // Obtiene la hora actual
                        def currentTime = new Date()
                        echo "Current time: ${currentTime}"
                        
                        // Agrega una hora y media a la hora actual
                        def newTime = new Date(currentTime.time + 1 * 60 * 1000) // Adding 90 minutes in milliseconds
                        echo "New time: ${newTime}"

                        // Define the date formatter
                        def sdf = new java.text.SimpleDateFormat("HH mm")
                        if (sdf) {
                            echo "sdf no es null"
                        } else {
                            echo "Error: sdf time is null"
                        } 

                        // Format the new time
                        def formattedTime = sdf.format(newTime)
                        echo "Formatted time: ${formattedTime}"

                        if (formattedTime) {
                            def hour = formattedTime.split(" ")[0]
                            def minute = formattedTime.split(" ")[1]
                            echo "Hour: ${hour}, Minute: ${minute}"
                        } else {
                            echo "Error: Formatted time is null"
                        } 

                        // Extract hour and minute from the formatted time
                        def hour = formattedTime.split(" ")[0]
                        def minute = formattedTime.split(" ")[1]
                        echo "Hour: ${hour}, Minute: ${minute}"

                        // Construye la expresión cron
                        def cronExpression = "${minute} ${hour} * * *"
                        echo "Cron expression: ${cronExpression}"

                        // Trigger the seed job
                        build job: 'automation/seedjob', parameters: [string(name: 'cron_value', value: cronExpression)], wait: true
                    } catch (Exception e) {
                        echo "An error occurred: ${e.message}"
                        throw e
                    }
                }
            }
        }
    }
}
            
