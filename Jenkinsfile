podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
        containers:
        - name: gradle 
          image: gradle:jdk8 
          command:
          - sleep
          args:
          - 99d
          restartPolicy: Never
    '''){
        node(POD_LABEL){
            stage('Build Services and Test'){ 
              git 'https://github.com/austineisele/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git' 
                container('gradle'){
                    stage('start calculator'){
                        sh '''
                        echo pwd
                        cd Chapter08/sample1
                        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                        chmod +x ./kubectl
                        ./kubectl apply -f calculator.yaml -n staging
                        ./kubectl apply -f hazelcast.yaml -n staging
                        sleep 30
                            '''
                    }
                    stage('test calculator'){
                        sh '''
                          curl -i calculator-service:8080/sum?a=6\\&b=2
                          curl -i calculator-service:8080/div?a=6\\&b=2    '''
                    } 
            }
      }
    }
}
