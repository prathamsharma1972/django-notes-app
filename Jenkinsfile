// @Library('Shared')_
// pipeline{
//     agent any
    
//     stages{
//         stage("Code clone"){
//             steps{
//                 sh "whoami"
//             clone("https://github.com/LondheShubham153/django-notes-app.git","main")
//             }
//         }
//         stage("Code Build"){
//             steps{
//             dockerbuild("notes-app","latest")
//             }
//         }
//         stage("Push to DockerHub"){
//             steps{
//                 dockerpush("dockerHubCreds","notes-app","latest")
//             }
//         }
//         stage("Deploy"){
//             steps{
//                 deploy()
//             }
//         }
        
//     }
// }
pipeline {
    agent any

    stages {

        stage("Check User") {
            steps {
                sh "whoami"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh "docker build -t notes-app:latest ."
            }
        }

        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHubCreds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    echo $PASS | docker login -u $USER --password-stdin
                    docker tag notes-app:latest $USER/notes-app:latest
                    docker push $USER/notes-app:latest
                    """
                }
            }
        }

        stage("Deploy") {
            steps {
                sh """
                docker stop notes-app || true
                docker rm notes-app || true
                docker run -d -p 8000:8000 --name notes-app $USER/notes-app:latest
                """
            }
        }
    }
}
