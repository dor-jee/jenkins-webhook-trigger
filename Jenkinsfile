pipeline {
  agent any
  tools {
    maven "maven-3.9"
  }

  stages {
    stage("test") {
      steps {
        echo "testing the application"
      }
    }

    stage("build") {
      when {
        expression {
          BRANCH_NAME == "main"
        }
      }
      steps {
        echo "building the application"
        sh "mvn -DskipTests package"

        withCredentials([
          usernamePassword(
            credentialsId: "dockerhub-credentials",
            usernameVariable: "USER",
            passwordVariable: "PASS"
          )
        ]) {
          echo "building and pushing Docker image"
          sh """
            docker build -t ndorjee/myapp:1.0 .
            echo "$PASS" | docker login -u "$USER" --password-stdin
            docker push ndorjee/myapp:1.0
          """
        }
      }
    }

    stage("deploy") {
      when {
        expression {
          BRANCH_NAME == "main"
        }
      }
      steps {
        echo "deploying the application"
      }
    }
  }
}
