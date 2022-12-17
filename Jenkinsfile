def region = 'eu-central-1'

node('master'){
    stage('Checkout'){
        checkout scm
    }

    stage('Authentication'){
        sh "aws eks update-kubeconfig --name petclinic-online-cluster --region ${region}"
    }

    stage('Deploy'){
        sh """
            helm dependency update todo
            helm upgrade --install todo ./todo -f values.override.yaml \
                --set metadata.jenkins.buildTag=${env.BUILD_TAG} \
                --set metadata.git.commitId=${getCommitId()}
        """
    }
}

def getCommitId() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}