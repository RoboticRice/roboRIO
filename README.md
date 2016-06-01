# roboRIO
All (personal) projects regarding the roboRIO (2015-current)

IntelliJ & git & jdk

Open IntelliJ
Create New Project -> Gradle -> Java (checked) -> NEXT
groupID = group (com.github.roboticrice)
artifactID = project name (roboRIO-Template) -> NEXT
Create directories...automatically (checked)
Use custom...(recommended) (checked) -> NEXT -> FINISH

open build.gradle in intelliJ, replace everything bellow "apply plugin: 'java'" with:

buildscript {
    repositories {
        mavenCentral()
        maven {
            name = "GradleRIO"
            url = "http://dev.imjac.in/maven"
        }
    }
    dependencies {
        classpath group: 'jaci.openrio.gradle', name: 'GradleRIO', version: '+'
    }
}

//apply plugin: 'eclipse'
apply plugin: 'GradleRIO'                                 //Apply the GradleRIO plugin

gradlerio.robotClass = "REPLACEME.MAIN"   //The class for the main org.usfirst.frc.team3863.Robot Class. Used in manifest
gradlerio.team = "3863"                                   //Your FRC team number (e.g. 5333 for team 'Can't C#', or 47 for Chief Delphi)
//gradlerio.rioIP = "10.53.33.20"                         //Uncomment to specify the IP address of the RIO

def robotManifest = {
//    attributes 'Main-Class': 'org.usfirst.frc.team3863.Robot'
    attributes 'Main-Class': 'edu.wpi.first.wpilibj.RobotBase'
    attributes 'Robot-Class': gradlerio.robotClass
}

jar {
    from configurations.compile.collect { it.isDirectory() ? it : zipTree(it) }
    manifest robotManifest
}

task genJavadoc(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}

artifacts {
    archives genJavadoc
}
repositories {
    mavenCentral()
}

/////////////////////////////////////////////////////////////////////////////
save and refresh gradle

new package->com.github.roboticrice
new java file->Main

replace "REPLACEME.MAIN" in build.gradle with "package name"."main file name".

file new file ".gitignore"
add:
.gradle/
.idea/
build/
*.iml

/////////////////////
VCS->Enable Version....->Git->OK
Add files using VCS drop down box
commit
push->select remote-> copy url from github