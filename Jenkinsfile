properties([
    [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobRestrictionProperty'],
    parameters([
        choice(name: 'environment_name', choices: ['lab','development', 'production'], description: 'Specify the environment'),
        string(name: 'source_branch', description: 'Tower as code source branch')
    ])
])
 
node(){
 
    println("env.environment_name = " + env.environment_name)
    println("env.source_branch = ${env.source_branch}")
    println("NODE_NAME = ${env.NODE_NAME}")
    println("env.GIT_BUILD_CAUSE = " + env.GIT_BUILD_CAUSE )
 
    // Tower Job To Run
    def job_number = 741
   
    sh 'mkdir -p .git && chmod -R u+w .git'
    unstash "workspace"
    sh 'chmod -R u+rwx .git'
 
    // Manual trigger lint, molecule test, then run tower as code
    linting()
    molecule_test()
 
 
   
               
   // withCredentials([string(credentialsId: 'bearerID', variable: 'TOKEN')]) {
    def response = httpRequest acceptType: 'APPLICATION_JSON',
    httpMode: 'GET', ignoreSslErrors: true,
    //  customHeaders: [[name: 'Authorization', value: 'Bearer $TOKEN']],
    customHeaders: [[name: 'Authorization', value: 'Bearer V5JcHBUdcFUVD31gafJIS3PaC4aZDF']],
    url: 'https://aap/api/v2/job_templates/${job_number}/launch/'
               
    println("Response is = " + response.content)
                            
    //TODO: Add tags and other parameters needed
    def body = """ {"scm_branch": "${env.source_branch}"}  """
               
    println(body)
    response = httpRequest acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON',
    httpMode: 'POST', ignoreSslErrors: true, customHeaders: [[name: 'Authorization', value: 'Bearer V5JcHBUdcFUVD31gafJIS3PaC4aZDF']],
    requestBody: body,
    url: 'https://aap/api/v2/job_templates/${job_number}/launch/'
 
 
 
 }
 
    // // on_commit
    // on_commit to: /^(.*)[Ff]eature(.*)/, {
    //  println(">>>> on_commit feature, so lint...")
    //  linting()
    // }
 
    // // on_pull_request
    // on_pull_request to: /^(.*)[Dd]ev(elop|elopment|eloper|)$/,{
    //  println(">>>> on_pull_request to dev, so lint...")
    //  linting()
    // }    
 
    // on_pull_request to: /^(.*)[Ss]table/,{
    //  println(">>>> on_pull_request to stable, so lint...")
    //  linting()
    //  // TODO: some level of integration required
    // }    
 
    // on_pull_request to: /^(.*)[Rr]elease$/,{
    //  println(">>>> on_pull_request to release, so lint...")
    //  linting()
    // }    
   
    // on_pull_request to: /^([Mm]ain|[Mm]aster)$/,{
    //  println(">>>> on_pull_request to main || master, so lint...")
    //  linting()
    // }
 
 
    // on_merge events
    // on_merge to: /^(.*)[Dd]ev(elop|elopment|eloper|)$/,{            
    //  linting()                  
    //  sh 'cat ./molecule/default/verify.yml'
    //  molecule_test()
    // }
 
    // on_merge to: /^(.*)[Ss]table/,{  
    //  linting()
 
    //  dir("merged"){
    //      molecule_test()
    //  }      
    // }            
 
    // on_merge to: /^(.*)[Rr]elease$/,{
    //  linting()
 
    //  dir("merged"){
    //      molecule_test()
    //  }      
    // }    
 
    // on_merge to: /^([Mm]ain|[Mm]aster)$/,{
    //  linting()
 
    //  dir("merged"){
    //      molecule_test()
    //  }      
    // }    

 

