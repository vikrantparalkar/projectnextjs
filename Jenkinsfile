node {
    def appDir = '/var/www/nextjs-app'
    
    stage('Clean Workspace') {
        echo 'Cleaning Jenkins workspace'
        deleteDir()
    }

    stage('Clone Repo') {
        echo 'Cloning the repo'
        git(
            branch: 'main',
            url: 'https://github.com/vikrantparalkar/projectnextjs'
        )
    }

    stage('Deploy to EC2') {
        echo 'Deploying to EC2'
        sh """
            # Create app directory
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}
            
            # Sync files (exclude unnecessary stuff)
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            # Go to app directory
            cd ${appDir}
            
            # Install dependencies & build
            npm install
            npm run build

            # Kill anything already running on port 3000
            sudo fuser -k 3000/tcp || true

            # Start the app with pm2 (runs in background)
            if ! command -v pm2 >/dev/null 2>&1; then
                sudo npm install -g pm2
            fi

            pm2 stop nextjs-app || true
            pm2 start "npm run start" --name nextjs-app
            pm2 save
        """
    }
}
