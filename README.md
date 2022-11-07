========
Commands
========
 
gradle init
gradle build --> to create artifact in the name of root project in build/libs/*.war
gradle docker
gradle dockerRun

============
build.gradle 
============
plugins { 
	id "war" 
	id "com.palantir.docker" version "0.22.1" 
	id "com.palantir.docker-run" version "0.22.1" 
}

docker { 
	name "gradle_image" // name  
	files 'build/libs/gradle_webapps.war' // or path to .war file 
}

dockerRun { 
	name "gradle_container" // name gradle_container 
	image "gradle_image" // image gradle_image 
	ports '80:8080' 
	clean true 
}

========== 
Dockerfile
========== 

FROM tomcat 
COPY *.war /usr/local/tomcat/webapps/gradle.war (no need to mention source path bcos gradle ne image banate time intermediate folder me daal diya tha, do gradle image banane vale task me path dedo taki vaha se utha paye and yaha directly mil jaye)

=======
Install
=======

apt upgrade -y
apt install docker.io gradle maven tree git -y
