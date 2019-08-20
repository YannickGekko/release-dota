podTemplate(containers: [
  containerTemplate(name: 'helm', image: 'alpine/helm:latest', command: 'cat', ttyEnabled: true)
  ], 
  envVars: [
    podEnvVar(key:'CHART_NAME', value: 'confluentinc/cp-helm-charts'), 
    podEnvVar(key:'NAMESPACE', value: 'dev'), 
  ]) {
  node(POD_LABEL){
    def test = 'export ohhl'
      withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_Dotaki_Preprod_Cred']]){
        stage('Git clone '){
          sh '''
          echo '######################## Deploy node-workers Start #################################'
          git clone https://github.com/loick-gekko/release-dota.git
          cd release-dota
          git checkout $BRANCH_NAME
          '''
        }
        stage('Deploy api-node-gekko '){
          container('helm'){
            sh '''
            helm init --client-only
            helm repo add confluentinc https://confluentinc.github.io/cp-helm-charts/
            helm repo update
            helm install $CHART_NAME --namespace $NAMESPACE --name $BRANCH_NAME -f values.yaml  || helm upgrade $BRANCH_NAME $CHART_NAME -f values.yaml
            echo '######################## Deploy node-workers End #################################'
            '''
          }
        }
      }
    }
}
