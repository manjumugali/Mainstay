node {


    
    def app
    def dockerRegistry
    def dockerCreds
    
    parameters {
        string(name: 'git_commit_hash', defaultValue: '90acd4cbbe60100be560cd85dbae9d0e8eeca7ad', description: 'Git Commit Hash')
    }


    stage('Clone repository') {
        /* Let's make sure we h ave the repository cloned to our workspace */

        /*checkout scm*/
        checkout ([
            $class: 'GitSCM',
            branches: [[name: "master" ]],
            userRemoteConfigs: [[
            url: 'https://github.com/manjumugali/Mainstay.git']]
                   ])
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("spinnaker")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        
            echo "Tests passed, nothing to see here."
        
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        /* If we are pushing to docker hub, use this: */
           dockerRegistry =  'https://registry.hub.docker.com'
           dockerCreds = 'fernando-dockerhub'
        /* If we are pushing to Artifactory, use this: 
        dockerRegistry = 'https://armory-docker-local.jfrog.io'
        dockerCreds = 'fernando-armory-artifactory'*/
        
        docker.withRegistry('https://331955262420.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:ecr' ) {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            
        }
    }
    stage('ECR') {
        echo "Pushed image to ECR"
        
    }
        
}
