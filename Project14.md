# Experience Continuous Integration With Jenkins | Ansible | Artifactory | Sonarqube | PHP

### Step 1
 I installed 'Blue Ocean' Pluggin (a tool for pipeline creation) in my Jenkins console.
 
 ![1  blue ocean install](https://user-images.githubusercontent.com/79808404/210576954-a0141c2d-408b-4edf-8778-e7672e5428fa.JPG)

![2  open blue ocean](https://user-images.githubusercontent.com/79808404/210577174-0cab7aac-b487-41b3-a4c3-e8a391717c95.JPG)

### Step 2
  I logon to my Github account to generate a token for my jenkins to authorize itself to github.
  
  ![3  git setting for token](https://user-images.githubusercontent.com/79808404/210578123-90027150-0b71-441a-b607-66d96bd36abb.JPG)


![4  generate token](https://user-images.githubusercontent.com/79808404/210578149-c5f52957-b132-4ae5-a73a-faf8228b933b.JPG)


![5  token](https://user-images.githubusercontent.com/79808404/210578178-5e7cdf62-259c-4207-96a3-8f774b97641c.JPG)

### Step 3
 I connected my token to my jenkins and created a pipeline
 
 ![6  token connected](https://user-images.githubusercontent.com/79808404/210578452-fa1ac9e0-9e99-4aba-aafb-3ce69797bb76.JPG)

![6 pipeline created](https://user-images.githubusercontent.com/79808404/210578466-ab804740-2c26-43c5-b714-f759f9997a34.JPG)


### Step 4
 I created a 'deploy' folder in my  'ansible-conf-mgt' and added a file 'Jenkinsfile' and input the below command in it.
 
       pipeline {
           agent any

         stages {
           stage('Build') {
             steps {
              script {
                sh 'echo "Building Stage"'
                }
              }
            }
            
            stage('Test') {
             steps {
              script {
                sh 'echo "Testing Stage"'
              }
            }
          }
          }
        }

 
 ![7 jenkinsfile added](https://user-images.githubusercontent.com/79808404/210580491-8081d1b1-1607-407f-8624-7b4b3804b115.JPG)
 
 I added the path to the jenkinsfile folder in my Jenkins configuration
 
 ![7 jenkins path](https://user-images.githubusercontent.com/79808404/210581005-a25c8792-5e1f-4b50-8e06-12b8917b56a8.JPG)

### Step 5
  I saved the setting and clicked on the pipeline created 'main'

![9  pipeline created](https://user-images.githubusercontent.com/79808404/210586204-4dbdc5a5-981d-41a1-b527-ebda22bd8c57.JPG)

  
 ![10  build now](https://user-images.githubusercontent.com/79808404/210586844-ca941ca7-ff6c-4581-9301-c3f38c2e81d0.JPG)

### Step 6
 I clicked on 'Build now' to be sure my configuration works as expected.
 
 
 ![10 build now 1b](https://user-images.githubusercontent.com/79808404/210587328-fbbd08d2-9ba9-4428-9ceb-50c5b6e8624d.JPG)
 
 ![10 build now 2](https://user-images.githubusercontent.com/79808404/210587400-c6dbda5a-3388-4b22-9f09-e7ab1f627cfb.JPG)

### Step 7
I created and switched to a new branch 'feature/jenkinspipeline-stages' on my github 
 
 ![git branch](https://user-images.githubusercontent.com/79808404/210625046-4f55beb8-4552-4eb4-b923-bac5d42cb63a.JPG)

### Step 8
 I added more stages in my jenkinsfile by inputing the below code snippet and pushed the changes to the new feature/jenkinspipeline-stages
  
  
 
           pipeline {
               agent any

             stages {
               stage('Build') {
                 steps {
                   script {
                     sh 'echo "Building Stage"'
                   }
                 }
               }

               stage('Package') {
                 steps {
                   script {
                     sh 'echo "Packaging Stage"'
                   }
                 }
               }

               stage('Deploy') {
                 steps {
                   script {
                     sh 'echo "Deploying Stage"'
                   }
                 }
               }
               stage('Clean up') {
                 steps {
                   script {
                     sh 'echo "Cleaning Up Stage"'
                   }
                 }
               }
               stage('Test') {
                steps {
                  script {
                    sh 'echo "Testing Stage"'
                  }
                }
              }
              }
            }


### Step 9
  I navigated to the scan repository button in my jenkins console to see the new branch created in my jenkins console
    
  ![1  feature](https://user-images.githubusercontent.com/79808404/210627490-c5b10b43-cb08-42ed-9277-c299d424d43e.JPG)

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
 
 
 
 
 
