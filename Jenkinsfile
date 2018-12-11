@Library('transitive-pipeline-library') _

withEnv([
    'ENVIRONMENT=dev',
    'project=depshield-testing/transitive-3'
  ]) {
  withCredentials([
    string(credentialsId: 'gitHubApiToken', variable: 'gitHubApiToken')
  ]) {
  node {
    stage('Build') {
//        def commitId = transitive.getCommitId()
//        transitive.postGitHub(commitId, 'pending', 'build', 'Build is running', '')

        try {
          bat  "mvn clean package"
//          transitive.postGitHub(commitId, 'success', 'build', 'Build succeeded', '')
        } catch (error) {
//          transitive.postGitHub(commitId, 'failure', 'build', 'Build failed', '')
          throw error
        }
    }

    stage('Nexus Lifecycle Analysis') {
//        def commitId = transitive.getCommitId()
//        transitive.postGitHub(commitId, 'pending', 'analysis', 'Nexus Lifecycle Analysis is running', '')

        try {
          def policyEvaluation = nexusPolicyEvaluation failBuildOnNetworkError: true, iqApplication: 'transitive-3', iqScanPatterns: [[scanPattern: 'mod-boot*.jar']], iqStage: 'build', jobCredentialsId: 'nexus-iq'
          def policyEvaluation2 = nexusPolicyEvaluation failBuildOnNetworkError: true, iqApplication: 'transitive-3', iqScanPatterns: [[scanPattern: 'mod-orm*.jar']], iqStage: 'build', jobCredentialsId: 'nexus-iq'
//          transitive.postGitHub(commitId, 'success', 'analysis', 'Nexus Lifecycle Analysis succeeded', "${policyEvaluation.applicationCompositionReportUrl}")
        } catch (error) {
//          def policyEvaluation = error.policyEvaluation
//          transitive.postGitHub(commitId, 'failure', 'analysis', 'Nexus Lifecycle Analysis failed', "${policyEvaluation.applicationCompositionReportUrl}")
          throw error
        }
    }
  }
  }
}
