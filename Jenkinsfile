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
    stage("Read POM"){
        script{
            def pomXml = readFile 'pom.xml'
            def pom = new XmlSlurper().parseText(pomXml)
            def groupId = pom.getGroupId()
            def artifactId = pom.getArtifactId()
            def version = pom.getVersion()  
            println "\n   groupId ==> ${groupId}"     
            println "\n   ArtifactID ==> ${artifactId}"  
            println "\n   Version ==> ${version}"  
}
    }
    println "\n\n#################"+
        "\nImageRegistry==> ${config.imageRegistry}\n"+
     "#################"
}