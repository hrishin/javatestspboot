@Library('github.com/hrishin/osio-pipeline@runtest-fix') _

osio {

  config runtime: 'java', version: '1.8'

  ci {
    echo "Test CI..."

     integrationTestCmd = "mvn verify integration-test -Dnamespace.use.current=false -Dnamespace.use.existing=${testNamespace()} -Dit.test=*IT -DfailIfNoTests=false -DenableImageStreamDetection=true -Popenshift,openshift-it"
     spawn image: "java", version: "1.8", commands: args.commands)
  }

  cd {
     echo "Test cd."
    // processing openshift template present in .openshiftio/application.yaml
    def resources = processTemplate(params: [
          RELEASE_VERSION: "1.0.${env.BUILD_NUMBER}"
    ])

    // performs an s2i build
    build resources: resources
    // deploy to stage environment
    deploy resources: resources, env: 'stage'
    // wait for user to approve the promotion to "run" environment
    deploy resources: resources, env: 'run', approval: 'manual'

  }
}
