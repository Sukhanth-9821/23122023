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
           def properties = readProperties file: './application-dev.properties'
           def groupId = properties.getProperty('group.id')
           def artifactId = properties.getProperty('artifact.id')
           def version = properties.getProperty('version')  
           println "\n\n group.id ==> ${groupId}"
           println "\n\n artifactId.id ==> ${artifactId}"
           println "\n\n version.id ==> ${version}"
}  
    }
    println "\n\n#################"+
        "\nImageRegistry==> ${config.imageRegistry}\n"+
     "#################"
}