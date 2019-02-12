@Library('github.com/piyush-garg/osio-pipeline@iss_74') _

osio {

  config runtime: 'java', version: '1.8'

  ci {
    echo "Test CI..."

     integrationTestCmd = "mvn verify integration-test -Dnamespace.use.current=false -Dnamespace.use.existing=${testNamespace()} -Dit.test=*IT -DfailIfNoTests=false -DenableImageStreamDetection=true -Popenshift,openshift-it"
     runTest commands: integrationTestCmd
  }

  cd {
     echo "Test cd."
    // processing openshift template present in .openshiftio/application.yaml
    def resources = processTemplate(params: [
          release_version: "1.0.${env.BUILD_NUMBER}"
    ])

    // performs an s2i build
    build resources: resources
    // deploy to stage environment
    deploy resources: resources, env: 'stage'
    // wait for user to approve the promotion to "run" environment
    deploy resources: resources, env: 'run', approval: 'manual'

  }
}
