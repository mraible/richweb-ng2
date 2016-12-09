node {
    def nodeHome = tool name: 'node-6.9.1', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
    env.PATH = "${nodeHome}/bin:${env.PATH}"

    stage('check tools') {
        sh "node -v"
        sh "npm -v"
    }

    stage('checkout') {
        checkout scm
    }

    stage('npm install') {
        sh "npm install"
    }

    stage('unit tests') {
        sh "ng test --watch false"
    }

    stage('protractor tests') {
        sh '''ng serve &
        sleep 15s
        npm run e2e
        '''
    }

    stage('deploying') {
        sh '''
        # exit 1 on errors
        set -e

        # deal with remote
        echo "Checking if remote exists..."
        if ! git ls-remote heroku; then
          echo "Adding heroku remote..."
          git remote add heroku https://git.heroku.com/rocky-bayou-25807.git
        fi

        # push only origin/master to heroku/master - will do nothing if
        # master doesn't change.
        echo "Updating heroku master branch..."
        git push heroku origin/master:master
        '''
    }
}
