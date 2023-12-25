node() {
    def config = [:]
    config.imageRegistry='12345'
    boolean MasterBranch = env.BRANCH_NAME.startsWith("master")

    stage("SCM"){
        checkout scm
    }
    stage("MVN Build"){
        if (MasterBranch){
            sh script: "mvn clean install package"
        }
    }
    stage("Read Nexus Values"){
        script{
            config.version = sh(script: 'grep \"^VERSION\" application-dev.properties | cut -d \"=\" -f 2 | tr -d "\n"| tr -d "\r"| tr -d " "',returnStdout: true)
            config.group_id = sh(script: 'grep \"^GROUP_ID\" application-dev.properties | cut -d \"=\" -f 2 | tr -d "\n"| tr -d "\r"| tr -d " "',returnStdout: true)
            config.artifact_id = sh(script: 'grep \"^ARTIFACT_ID\" application-dev.properties | cut -d \"=\" -f 2 | tr -d "\n"| tr -d "\r"| tr -d " "',returnStdout: true)


           
}  
    }
    println "\n\n#################"+
        "\nImageRegistry==> ${config.imageRegistry}\n"+
         "\n\n version.id ==> ${config.version}"+
         "\n\n group_id ==> ${config.group_id}"+
         "\n\n artifact_id ==> ${config.artifact_id}"+
     "\n#################"
     stage("Nexus Push"){
         if (MasterBranch){
           script{
              // Nexus Repository Manager URL
                NEXUS_URL="http://13.200.252.234:8081/repository/Project-Production/"

               // Nexus Repository credentials
                NEXUS_USERNAME="sukhanth"
                NEXUS_PASSWORD=India@#123"

                // Path to the WAR file
                WAR_FILE_PATH="/webapp/target/webapp.war"

                // Artifact group, artifact id, version, and packaging
                GROUP_ID="${config.group_id}"
                ARTIFACT_ID="${config.artifact_id}"
                VERSION="${config.version}"
                PACKAGING="war"

                // Nexus Repository endpoint for deploying artifacts
                DEPLOY_ENDPOINT="/service/rest/v1/components?repository=Project-Production"

                // Constructing the URL for deployment
                DEPLOY_URL="${NEXUS_URL}${DEPLOY_ENDPOINT}"

               // Deploying the WAR file to Nexus Repository
                curl -v -u "${NEXUS_USERNAME}:${NEXUS_PASSWORD}" --upload-file "${WAR_FILE_PATH}" \
                "${DEPLOY_URL}&maven.groupId=${GROUP_ID}&maven.artifactId=${ARTIFACT_ID}&maven.version=${VERSION}&maven.asset1.extension=${PACKAGING}"

               // Check the HTTP response to ensure successful deployment
               // You can add additional error handling based on the response status


           }

                   
            
         }
     }
}