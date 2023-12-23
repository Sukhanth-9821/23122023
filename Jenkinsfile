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
           println "\n\n version.id ==> ${config.version}"

           
}  
    }
    println "\n\n#################"+
        "\nImageRegistry==> ${config.imageRegistry}\n"+
     "#################"
}