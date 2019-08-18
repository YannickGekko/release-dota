podTemplate(containers: [
  containerTemplate(name: 'helm', image: 'alpine/helm:latest', command: 'cat', ttyEnabled: true)
  ], 
  envVars: [
    podEnvVar(key:'CHART_NAME', value: 'stable/prometheus'), 
    podEnvVar(key:'NAMESPACE', value: 'admin'), 
  ]) {
  node(POD_LABEL){
    def test = 'export ohhl'
      withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_Dotaki_Preprod_Cred']]){
        stage('Git clone '){
          sh '''
          echo '######################## Deploy Prometheus Chart Start #################################'
          git clone https://github.com/loick-gekko/release-dota.git
          cd dotaki-api-node
          git checkout $BRANCH_NAME
          '''
        }
        stage('Deploy Prometheus Chart '){
          container('helm'){
            sh '''
            helm init --client-only
            helm repo update
            helm install $CHART_NAME --namespace $NAMESPACE --name $BRANCH_NAME || helm upgrade $BRANCH_NAME $CHART_NAME
            echo '######################## Deploy node-workers End #################################'
            '''
          }
        }
      }
    }
}
