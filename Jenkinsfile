node{
    stage('git checkout'){
//         updateGitlabCommitStatus name: 'jenkins', state: 'pending'
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'se-ssh-private', url: 'git@github.com:HotWaterMore/HotWaterFrontend.git']]])
        echo '=== git clone end ==='
    }

    stage('install'){
//         updateGitlabCommitStatus name: 'jenkins', state: 'running'
        sh label: 'build', returnStatus: true, script: 'cnpm install'
        echo '=== cnpm install end ==='
    }

    stage('build'){
//         updateGitlabCommitStatus name: 'jenkins', state: 'running'
        sh label: 'build', returnStatus: true, script: 'cnpm run build'
        echo '=== cnpm run build end ==='
    }

    stage('zip'){
//         updateGitlabCommitStatus name: 'jenkins', state: 'running'
        sh label: 'build', returnStatus: true, script: 'tar -zcvf HotWaterFrontend.tar.gz dist'
        echo '=== cnpm run build end ==='
    }

// 下面这个是远程部署到非jenkins所在的服务器上，本项目在本机部署
    def remote = [:]
        remote.name = 'jenkins'
        remote.host = '172.19.241.40'
        remote.user = 'jh'
        remote.password = 'jhqwerdf'
        remote.allowAnyHosts = true
    stage('deploy'){
// 		updateGitlabCommitStatus name: 'jenkins', state: 'running'
        sshPut remote: remote, from: 'HotWaterFrontend.tar.gz', into: '/home/jh/artifacts/'
        sshCommand remote: remote, command: """
            cd artifacts
            ./deploy_frontend.sh
        """
        echo '=== deploy end ==='
	}

    stage('archive'){
//         updateGitlabCommitStatus name: 'jenkins', state: 'success'
        archiveArtifacts allowEmptyArchive: true, artifacts: 'HotWaterFrontend.tar.gz', onlyIfSuccessful: true
    }
}
