pipeline { 
    agent any

    stages {
	
        stage('CLONE GITHUB CODE') {
            steps {
                echo 'In this stage code will be cloned'
               git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/priyankayadav-911/2-ansible-python-project.git'
            }
        }
		
        stage('BUILDING THE CODE') {
            steps {
                echo 'In this stage code will be built and mvn artifact will be generated'
                sh 'mvn clean install'
            }
        }	
        stage('Check Artifact') {
           steps {
               sh '''
               echo "Workspace: $WORKSPACE"
               ls -l $WORKSPACE/target
               '''
           }
        }
        stage('Deploy with Ansible') {
            steps {
              sh '''
                 ARTIFACT=$(ls $WORKSPACE/target/*.war | head -n 1)
                 echo "Deploying artifact: $ARTIFACT"
                 /usr/bin/ansible-playbook -i ansible/inventory ansible/deploy_tomcat.yml \
                --extra-vars "artifact=$WORKSPACE/target/devops-3.2.0.war"
                 '''
             }
        }
    }
}
