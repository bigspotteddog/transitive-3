@Library('transitive-pipeline-library') _

withEnv([
    'ENVIRONMENT=dev',
    'project=bigspotteddog/transitive-3'
  ]) {
  node {
    stage('Build') {
        try {
          sh  "mvn clean package"
        } catch (error) {
          throw error
        }
    }

    stage('Nexus Lifecycle Analysis') {
        try {
          def policyEvaluation = nexusPolicyEvaluation failBuildOnNetworkError: true, iqApplication: 'T3A', iqScanPatterns: [[scanPattern: '**/target/mod-boot*dependencies.jar']], iqStage: 'build', jobCredentialsId: 'nexus-iq'
          def policyEvaluation2 = nexusPolicyEvaluation failBuildOnNetworkError: true, iqApplication: 'T3B', iqScanPatterns: [[scanPattern: '**/target/mod-orm*dependencies.jar']], iqStage: 'build', jobCredentialsId: 'nexus-iq'
        } catch (error) {
          throw error
        }
    }

    stage('Nexus Lifecycle Analysis II') {
        try {
          def policyEvaluation = nexusPolicyEvaluation failBuildOnNetworkError: true, iqApplication: 'T3C', iqScanPatterns: [[scanPattern: '**/target/mod-xml*dependencies.jar']], iqStage: 'release', jobCredentialsId: 'nexus-iq'
        } catch (error) {
          throw error
        }
    }
  }
}
