node{
    def appDir ='/var/www/nextjs-app'
    
    stage('Clean Workspace'){
        echo 'cleaning jenkins workspace'
        deleteDir()
    }
    stage('Clone Repo'){
        echo 'clonning the repo'
        git(
            branch: 'main',
            url: 'https://github.com/vikrantparalkar/projectnextjs'
        )
    }
    stage('Deply to EC2'){
        echo 'deploying to ec2'
       
            sh"""
            sudo mkdir -p {appDir}
            sudo chown -R jenkins:jenkins ${appDir}
            
            rsync -av --delete --exclude='.git' --exclude= 'node_modules' ./ ${appDir}

            cd ${appDir}
            sudo npm install
            sudo npm run build
            sudo fuser -k 3000/tcp || true
            npm run start


            """
        
    }
}