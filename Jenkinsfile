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
     "#################"
}