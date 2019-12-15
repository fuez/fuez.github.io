---
title: gradle
date: 2017-07-06T12:05:25+08:00
lastmod: 2017-07-25T09:09:25+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

### Tips
## How to create an empty java project?
Run the following command:
`gradle init --type java-library`

## How to publish to local maven?
Add the following script to build.gradle file:
```
apply plugin: 'maven-publish
group="com.ibm.crl.bc"
publishing {
    publications {
        mavenJava(MavenPublication) {
            from components.java
        }
    }
}
```
## How to run junit test cases paralelly?
Here is a [session](https://discuss.gradle.org/t/how-to-execute-junit-tests-in-parallel/19308/3) about this topic. The fork is based on test class, to increase paralell, need to split test cases to test classes.
There is a method called [Test.setMaxParallelForks(int)](https://docs.gradle.org/3.0/javadoc/org/gradle/api/tasks/testing/Test.html#setMaxParallelForks(int))
