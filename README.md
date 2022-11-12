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

================
Generic Commands
================
build.gradle

plugins {
    id "war"
    id "com.palantir.docker" version "0.22.1"
    id "com.palantir.docker-run" version "0.22.1" 
}

version '0.1.0' // not necessary i think

docker {
     name "${project.name}:${project.version}" // name gradle_image
     files 'challenge.war' // or path to challenge.war
}
// challenge.war is the name of the artifact created
dockerRun {
    name "${project.name}" // name gradle_container
    image "${project.name}:${project.version}" // image gradle_image
    ports '8003:8080'
    clean true
}


=================================================================================
Dockerfile

FROM tomcat
COPY build/libs/*.war /usr/local/tomcat/webapps/gradle.war

===================================================================================
index.jsp

<html>
<body>
  <h2>This is deployment</h2>
</body>
</html>

========================================================================================
root project name : challenge

build/libs/challenge.war




For multi project

Step1: create normal project which will serve as root. Create a directory, cd it, and hit gradle init, select basic project
Step2: create a directory inside root lets say its name is child and it will have same contents as of parent i.e. src and build.gradle (src/main/webapp/index.jsp)
Step 3: go in root settings.xml file and below root project name add, include 'child'
Step 4: in root, hit gradle projects and now you will se child is recognised as subproject
Step 5: in parent settings.xml add subprojects{ apply plugin: 'war'}