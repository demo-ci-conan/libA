def artifactory_name = "Artifactory Docker"
def artifactory_repo = "hackathonv5-build"
def repo_url = 'https://github.com/conan-community/conan-zlib.git'
def repo_branch = "release/1.2.11"

node {
    docker.image("conanio/gcc8").inside("--net=host") {    
      withEnv(["CONAN_USER_HOME=${env.WORKSPACE}/conan_cache"]) {
        
        def server = Artifactory.server artifactory_name
        def client = Artifactory.newConanClient()
        def serverName = client.remote.add server: server, repo: artifactory_repo
        stage("Get recipe")
        { 
            git branch: repo_branch, url: repo_url 
            
        }
        stage("Test recipe")
        { 
            client.run(command: "create . demo/testing" ) 
        }
        stage("Upload packages")
        {
            def buildInfo = Artifactory.newBuildInfo()
            //buildInfo.name = 'test:features:test'
            //buildInfo.number = 'v1.2.10'
            //String command = "upload * --all -r ${serverName} --confirm"
            
            //def b = client.run(command: command, buildInfo: buildInfo)
            
            server.publishBuildInfo buildInfo
        }
      }
    }
}