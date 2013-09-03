## Quarks that are solved

Most of this is pretty normal pulled mostly from [here](http://www.gradle.org/docs/current/userguide/multi_project_builds.html) page. You are going to need to add the following to your `module/build.gradle`:

    evaluationDependsOn(':project1')
    evaluationDependsOn(':project2')

This means that gradle will evaluate `:project1` and `:project2` before `:module`. 

## Setup
Every project is going to need at least a blank `build.gradle`.

We are going to need 4 `build.gradle`'s and 1 `settings.gradle`

## Required Files

All file paths are from AndroidTopProject

#### build.gradle

You are going to need files here:

    build.gradle
    ../project1/build.gradle
    ../project2/build.gradle
    module/build.gradle

#### settings.gradle
    settings.gradle

## Root project setup

### build.gradle

    dependencies {
        project(":module")
    }

### settings.gradle    

    include ':module'
    
    include ':project1', ':project1:A1', ':project1:B1', ':project1:Z1'
    project(':project1').projectDir = new File(settingsDir, '../project1')
    project(':project1:A1').projectDir = new File(settingsDir, '../project1/A1')
    project(':project1:B1').projectDir = new File(settingsDir, '../project1/B1')
    project(':project1:Z1').projectDir = new File(settingsDir, '../project1/Z1')
    
    include ':project2', ':project2:A2', ':project2:B2', ':project2:Z2'
    project(':project2').projectDir = new File(settingsDir, '../project2')
    project(':project2:A2').projectDir = new File(settingsDir, '../project2/A2')
    project(':project2:B2').projectDir = new File(settingsDir, '../project2/B2')
    project(':project2:Z2').projectDir = new File(settingsDir, '../project2/Z2')

### ../project1/build.gradle 

    apply plugin: 'java'

    subprojects {
        apply plugin: 'java'

        sourceCompatibility = JavaVersion.VERSION_1_6
        targetCompatibility = JavaVersion.VERSION_1_6

        repositories{
            mavenCentral()
        }   

        //Anything else you would need here that would be shared across all subprojects
    }


### ../project2/build.gradle

    buildscript {
        repositories {
            mavenCentral()
        }   

        dependencies {
            classpath 'com.android.tools.build:gradle:0.5.+'
        }   
    }

    subprojects {
        apply plugin: 'android-library'

        android {
            compileSdkVersion 17
            buildToolsVersion "17.0"
        }   

        sourceCompatibility = JavaVersion.VERSION_1_6
        targetCompatibility = JavaVersion.VERSION_1_6

        repositories{
            mavenCentral()
        }   

        //Anything else you would need here that would be shared across all subprojects
    }

### module/build.gradle

    buildscript {
        repositories {
            mavenCentral()
        }   

        dependencies {
            classpath 'com.android.tools.build:gradle:0.5.+'
        }   
    }

    evaluationDependsOn(':project1')
    evaluationDependsOn(':project2')

    apply plugin: 'android'

    android {
        compileSdkVersion 17
        buildToolsVersion "17.0"
    }

    dependencies {
        compile project(":project1:A1")
        compile project(":project1:B1")
        compile project(":project1:Z1")

        compile project(":project2:A2")
        compile project(":project2:B2")
        compile project(":project2:Z2")
    }
