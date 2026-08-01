pipeline {
  agent any
  tools {
    maven "maven-3.9"
  }

  stages {
    stage("test") {
      steps {
        echo "testing the application"
        echo "webhook-trigger for multi-branch"
      }
    }

    stage("build") {
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
      steps {
        echo "deploying the application"
      }
    }
  }
}
