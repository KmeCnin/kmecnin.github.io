---
layout: post
title:  "How to create custom PhpStorm plugins"
date:   2018-11-15 10:56:45 +0100
categories: how-to
published: true
---

## Resources
- [PhpStorm Plugin Development](https://www.jetbrains.org/intellij/sdk/docs/phpstorm/phpstorm.html)
- [Getting Started with Gradle](https://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/prerequisites.html)
- [Gradle Intellij Plugin Documentation](https://github.com/JetBrains/gradle-intellij-plugin)

## Setup IDE

### Download
To be able to create IntelliJ (included PhpStorm) plugins you first need to download [IntelliJ IDEA Community edition](https://www.jetbrains.com/idea/download/#section=linux).

### First configuration
At the first launch of IntelliJ IDEA, you will be asked what default plugins you want to use.  
Ensure that **Gradle** is selected in *Build Tools* and **Plugin DevKit** is enabled in *Plugin Development*.foo

### Install JDK
Be sure to have a JDK installed in order to run Java.  

For Linux, just run the following commands:
- `sudo apt-get install openjdk-8-jre`
- `sudo apt-get install openjdk-8-jdk`

See full documentation: [OpenJDK - How to download and install prebuilt OpenJDK packages](https://openjdk.java.net/install/index.html).

### Install Gradle
You can directly install Gradle using apt-get:
1. `sudo apt install gradle`
2. `gradle cleanIdea idea`

## Create a new plugin
Follow this documentation about how to create your first plugin using Gradle:  
[JetBrains - Getting Started with Gradle](https://www.jetbrains.org/intellij/sdk/docs/tutorials/build_system/prerequisites.html)

### Add support of PHP libraries
In order to write a plugin for PhpStorm, you need to import `com.jetbrains.php`.  
In `build.gradle` add the following:
```
intellij {
    version '2018.2.6'
    plugins = ['coverage', 'com.jetbrains.php:182.4892.16']
}
```
Look for a version of the plugin that match your IDE requirements: [List of releases](https://plugins.jetbrains.com/plugin/6610-php).

Then require the PHP library in your `src/main/resources/META-INF/plugin.xml` by adding the following lines:
```
<depends>com.jetbrains.php</depends>
<depends>com.intellij.modules.platform</depends>
```

### Test your configuration
If everything is well setup, you should be able to run `Gradle > Tasks > intellij > verifyPlugin` in the Gradle window (tab at the very right of IDE window).

### Test your plugin using PhpStorm
Default behavior of Gradle when you do `Gradle > Tasks > intellij > runIde` in order to test your plugin, is to launch another instance of IntelliJ.  

In order to launch PhpStorm instead, you have to add this task in your `build.gradle`:
```
task runPhpStorm(type: org.jetbrains.intellij.tasks.RunIdeTask) {
    ideaDirectory '/absolute/path/to/your/PhpStorm/installation'
}
```
Then you are able to test your plugin with PhpStorm using `Gradle > Tasks > intellij > runPhpStorm`.
