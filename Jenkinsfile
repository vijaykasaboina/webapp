stage 'build'
node ('master'){
     git 'https://github.com/venkat09docs/webapp.git'
     withEnv(["PATH+MAVEN=${tool 'maven3.6.0'}/bin"]) {
          sh "mvn clean package"		  
	  echo "deploy the applicatio from the branch2"
     }
     stash excludes: 'target/', includes: '**', name: 'source'
}
stage 'test'
parallel 'integration': { 
     node ('master') {
          unstash 'source'
          withEnv(["PATH+MAVEN=${tool 'maven3.6.0'}/bin"]) {
               sh "mvn clean package"
	       echo "this is form branch 2"
          }
		  //
     }
}, 'functional': {
     node ('master') {
          unstash 'source'
          withEnv(["PATH+MAVEN=${tool 'maven3.3'}/bin"]) {
               sh "mvn clean package" //sonar:sonar
          }
     }
}
stage name:'deploy', concurrency: 1
node ('master') {
     unstash 'source'
     withEnv(["PATH+MAVEN=${tool 'maven3.6.0'}/bin"]) {
          sh "mvn clean package"
	  echo "deploy the applicatio from the branch2"
     }
archiveArtifacts '**/*.war'
}
