@Library('your-shared-library') _

import org.jfrog.hudson.pipeline.Artifactory


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
            //  def filesByGlob = findFiles(glob: "target/*.war")
            //  println "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
            //  def artifactPath = filesByGlob[0].path
            //  println "${artifactPath}"
            //  artifactExists = fileExists artifactPath
            //  println "${artifactExists}"
            withCredentials([usernamePassword(credentialsId: 'Nexus_Cred', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]){
                def server = Artifactory.server('http://13.200.252.234:8081/')  // Adjust to match your Nexus server ID
                def buildInfo = Artifactory.newBuildInfo()
                server.mavenDeployer {
                            repository('maven-snapshots') {
                                // Set appropriate values based on your project
                                pom.version = '${config.version}'
                                pom.artifactId = '${config.artifact_id}'
                                pom.groupId = '${config.group_id}'
                                println '*****IN _ HERE ***************'
                            }
                            deployerCredentialsId = 'Nexus_Cred'
                        }
            }
         }
     }
}