// Set your project Prefix using your GUID
def prefix      = "openjdk"

// Set variable globally to be available in all stages
// Set Maven command to always include Nexus Settings
def mvnCmd      = "mvn -s ./nexus_openshift_settings.xml"
// Set Development and Production Project Names
def devProject  = "dev"
def prodProject = "prod"
// Set the tag for the development image: version + build number
def devTag      = "0.0-0"
// Set the tag for the production image: version
def prodTag     = "0.0"

pipeline {
  agent {
      label "maven"
  }
  stages {
    // Checkout Source Code and calculate Version Numbers and Tags
    stage('Checkout Source') {
      steps {
       git url: "https://github.com/mm7259/openshift-quickstarts.git"
       checkout scm

       script {
          def pom = readMavenPom file: 'pom.xml'
          def version = pom.version

          // TBD: Set the tag for the development image: version + build number.
          // Example: def devTag  = "0.0-0"

          // TBD: Set the tag for the production image: version
          // Example: def prodTag = "0.0"

        }
      }
    }

    // Using Maven build the war file
    // Do not run tests in this step
    stage('Build War File') {
      steps {
        echo "Building version ${devTag}"
        sh "mvn clean package"
        // TBD

      }
    }

    // Using Maven run the unit tests
    stage('Unit Tests') {
      steps {
        echo "Running Unit Tests"

        // TBD
      }
    }

    //Using Maven call SonarQube for Code Analysis
    stage('Code Analysis') {
      steps {
        echo "Running Code Analysis"

        // TBD

      }
    }

    // Publish the built war file to Nexus
    stage('Publish to Nexus') {
      steps {
        echo "Publish to Nexus"

        // TBD

      }
    }

    // Build the OpenShift Image in OpenShift and tag it.
    stage('Build and Tag OpenShift Image') {
      steps {
        echo "Building OpenShift container image tasks:${devTag}"

        // TBD: Start binary build in OpenShift using the file we just published.
        // Either use the file from your
        // workspace (filename to pass into the binary build
        // is openshift-tasks.war in the 'target' directory of
        // your current Jenkins workspace).
        // OR use the file you just published into Nexus:
        // "--from-file=http://nexus.${prefix}-nexus.svc.cluster.local:8081/repository/releases/org/jboss/quickstarts/eap/tasks/${prodTag}/tasks-${prodTag}.war"


        // TBD: Tag the image using the devTag.

      }
    }

    // Deploy the built image to the Development Environment.
    stage('Deploy to Dev') {
      steps {
        echo "Deploy container image to Development Project"

        // TBD: Deploy the image
        // 1. Update the image on the dev deployment config
        // 2. Update the config maps with the potentially changed properties files
       // 3. Reeploy the dev deployment
       // 4. Wait until the deployment is running
       //    The following code will accomplish that by
       //    comparing the requested replicas
       //    (rc.spec.replicas) with the running replicas
       //    (rc.status.readyReplicas)
       //

      // def dc = openshift.selector("dc", "tasks").object()
      // def dc_version = dc.status.latestVersion
      // def rc = openshift.selector("rc", "tasks-${dc_version}").object()

      // echo "Waiting for ReplicationController tasks-${dc_version} to be ready"
      // while (rc.spec.replicas != rc.status.readyReplicas) {
      //   sleep 5
      //   rc = openshift.selector("rc", "tasks-${dc_version}").object()
      // }

      }
    }

    // Run Integration Tests in the Development Environment.
    stage('Integration Tests') {
      steps {
        echo "Running Integration Tests"
        script {
          def status = "000"

          // Create a new task called "integration_test_1"
          echo "Creating task"
          // The next bit works - but only after the application
          // has been deployed successfully
          // status = sh(returnStdout: true, script: "curl -sw '%{response_code}' -o /dev/null -u 'tasks:redhat1' -H 'Content-Length: 0' -X POST http://tasks.${prefix}-tasks-dev.svc.cluster.local:8080/ws/tasks/integration_test_1").trim()
          // echo "Status: " + status
          // if (status != "201") {
          //     error 'Integration Create Test Failed!'
          // }

          echo "Retrieving tasks"
          // TBD: Implement check to retrieve the task

          echo "Deleting tasks"
          // TBD: Implement check to delete the task

        }
      }
    }

    // Copy Image to Nexus Container Registry
    stage('Copy Image to Nexus Container Registry') {
      steps {
        echo "Copy image to Nexus Container Registry"
        // TBD. Use skopeo to copy


        // TBD: Tag the built image with the production tag.

      }
    }

    // Blue/Green Deployment into Production
    // -------------------------------------
    // Do not activate the new version yet.
    stage('Blue/Green Production Deployment') {
      steps {
        echo "Blue/Green Deployment"

        // TBD: 1. Determine which application is active
        //      2. Update the image for the other application
        //      3. Deploy into the other application
        //      4. Update Config maps for other application
        //      5. Wait until application is running
        //         See above for example code

      }
    }

    stage('Switch over to new Version') {
      steps {
        // TBD: Stop for approval


        echo "Executing production switch"
        // TBD: After approval execute the switch

      }
    }
  }
}

