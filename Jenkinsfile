node {
   stage('Preparation') {
      def scmData = checkout([
        $class: 'GitSCM',
        branches: [[name: 'master']],
        doGenerateSubmoduleConfigurations: false,
        submoduleCfg: [],
        userRemoteConfigs: [[url: 'git@github.com:praqma/promote-nexus-artifact-gilded-rose.git']]
      ])

      script {
          sh 'echo $PWD'
          sh 'ls -aln'
          echo "\nUnmodified pom.xml file:\n"
          sh 'cat pom.xml'
          def pomFile = readMavenPom(file: 'pom.xml')
          if (pomFile.version.endsWith('-SNAPSHOT')) {
              echo "\n\nThis IS a SNAPSHOT version!\n\n"
              def currentDateTime = new Date()
              String timeStamp = currentDateTime.format("yyyy.MM.dd'T'HH.mm.ss.S")
              String commitSha1 = scmData.GIT_COMMIT.trim()
              pomFile.version = "${pomFile.version.replace('-SNAPSHOT', '')}-b${env.BUILD_NUMBER}-${timeStamp}-${commitSha1}"
              writeMavenPom(file: 'pom.xml', model: pomFile)
          } else{
              echo "\n\nNot a SNAPSHOT version.\n\n"
          }
          stash (name: 'metadataFile', includes: 'pom.xml')
          echo "\nStashed pom.xml file:\n"
          sh 'cat pom.xml'
      }
   }
   stage('Build') {
      unstash 'metadataFile'
      sh 'mvn --settings ./m2/settings.xml --batch-mode install'
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Javadoc') {
      unstash 'metadataFile'
      sh 'mvn --settings ./m2/settings.xml --batch-mode site'
      archive 'target/site/**/*'
   }
   stage('Nexus Upload') {
      unstash 'metadataFile'
      sh 'mvn --settings ./m2/settings.xml --batch-mode deploy'
      archive 'target/site/**/*'
   }
   stage('Promote to BETA') {
      input "Deploy to BETA?"
      deleteDir()
      def scmData = checkout([
          $class: 'GitSCM',
          branches: [[name: 'master']],
          doGenerateSubmoduleConfigurations: false,
          extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'promotionDir']],
          submoduleCfg: [],
          userRemoteConfigs: [[url: 'git@github.com:praqma/promote-nexus-artifact.git']]
        ])
      dir('promotionDir') {
        unstash 'metadataFile'
        sh 'echo $PWD'
        sh 'ls -aln'
        sh './gradlew publish --project-prop targetEnvironment=beta --project-prop pomFile=pom.xml'
      }
   }
   stage('Promote to GAMMA') {
      input "Deploy to GAMMA?"
      deleteDir()
      def scmData = checkout([
          $class: 'GitSCM',
          branches: [[name: 'master']],
          doGenerateSubmoduleConfigurations: false,
          extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'promotionDir']],
          submoduleCfg: [],
          userRemoteConfigs: [[url: 'git@github.com:praqma/promote-nexus-artifact.git']]
        ])
      dir('promotionDir') {
        unstash 'metadataFile'
        sh 'echo $PWD'
        sh 'ls -aln'
        sh './gradlew publish -P targetEnvironment=gamma -P pomFile=pom.xml'
      }
   }
  stage('Promote to DELTA') {
      input "Deploy to DELTA?"
      deleteDir()
      def scmData = checkout([
          $class: 'GitSCM',
          branches: [[name: 'master']],
          doGenerateSubmoduleConfigurations: false,
          extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'promotionDir']],
          submoduleCfg: [],
          userRemoteConfigs: [[url: 'git@github.com:praqma/promote-nexus-artifact.git']]
        ])
      dir('promotionDir') {
        unstash 'metadataFile'
        sh 'echo $PWD'
        sh 'ls -aln'
        sh './gradlew publish -P targetEnvironment=delta -P pomFile=pom.xml'
      }
  }
  stage('Promote to EPSILON') {
      input "Deploy to EPSILON?"
      deleteDir()
      def scmData = checkout([
          $class: 'GitSCM',
          branches: [[name: 'master']],
          doGenerateSubmoduleConfigurations: false,
          extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'promotionDir']],
          submoduleCfg: [],
          userRemoteConfigs: [[url: 'git@github.com:praqma/promote-nexus-artifact.git']]
        ])
      dir('promotionDir') {
        unstash 'metadataFile'
        sh 'echo $PWD'
        sh 'ls -aln'
        sh './gradlew publish -P targetEnvironment=epsilon -P pomFile=pom.xml'
      }
  }
}
