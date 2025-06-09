pipeline {
    agent any 
        stages {
            
            stage ('Avvio container') {
                steps {
                    script { sh """
                                sudo docker run -d --name ubuntu-container -p 8080:22 alessandrotofani/track3_step2:2.1 || docker run -d --name ubuntu-container -p 8080:8080 ubuntu:latest 2> /dev/null || true
                                sudo docker ps -a
                                
                            """
                    }
                }
            }
        }
}