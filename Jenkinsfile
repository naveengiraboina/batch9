stage 'build'
node ('Slave1'){
     git 'git@github.com:venkat09docs/batch8.git'
     withEnv(["PATH+MAVEN=${tool 'maven3.3.3'}/bin"]) {
          sh "mvn clean package"
     }
     stash excludes: 'target/', includes: '**', name: 'source'
}
stage 'test'
parallel 'integration': { 
     node ('Slave1') {
          unstash 'source'
          withEnv(["PATH+MAVEN=${tool 'Maven3.3.3'}/bin"]) {
               sh "mvn clean package"
          }
		  //
     }
}, 'quality': {
     node ('Slave 1') {
          unstash 'source'
          withEnv(["PATH+MAVEN=${tool 'Maven3.3.3'}/bin"]) {
               sh "mvn clean package" //sonar:sonar
          }
     }
}
stage 'approve'
timeout(time: 7, unit: 'DAYS') {
     input message: 'Do you want to deploy?', submitter: 'admin'
}
stage name:'deploy', concurrency: 1
node ('Slave 1') {
     unstash 'source'
     withEnv(["PATH+MAVEN=${tool 'Maven3.3.3'}/bin"]) {
          sh "mvn clean package"
     }
}