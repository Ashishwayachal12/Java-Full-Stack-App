pipeline{
    agent {
        label 'test-server'
    }
    environment{
        REPOSITORY = "ashishwayachal12/java-full-stack"
    }
    stages{
        stage("checkout"){
            steps{
                git 'https://github.com/Ashishwayachal12/Java-Full-Stack-App.git'
            }
        }
        stage("create docker images"){
            steps{
                sh '''
                    docker build -t $REPOSITORY:frontend-v$BUILD_ID frontend/.
                    docker build -t $REPOSITORY:backend-v$BUILD_ID backend/.
                '''
            }
        }
        stage("push images"){
            steps{
                script{
                    withDockerRegistry(credentialsId: '1de534cb-b3d2-40ee-abf7-1d9119c165ad', url: 'https://index.docker.io/v1/') {
                        sh 'docker push $REPOSITORY:frontend-v$BUILD_ID'
                        sh 'docker push $REPOSITORY:backend-v$BUILD_ID'
                    }
                }
            }
        }
        stage("stopping old containers"){
            steps{
                 sh '''
            docker stop frontend || true
            docker rm frontend || true

            docker stop backend || true
            docker rm backend || true
        '''
            }
        }
        stage("create containers"){
            steps{
                sh '''
                    docker run -d --name frontend -p 3000:80 $REPOSITORY:frontend-v$BUILD_ID
                    docker run -d --name backend -p 9090:8080 $REPOSITORY:backend-v$BUILD_ID 
                '''
            }
        }
        // stage("clean docker"){
        //     steps{
        //         sh 'echo y | docker system prune -a'
        //     }
        // }
        
        }
    }
