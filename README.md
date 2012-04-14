# JavaCC Compiler Plugin for Gradle 

Provides the ability to use [JavaCC](http://javacc.java.net/) and [JJTree](http://javacc.java.net/doc/JJTree.html) 
via [Gradle](http://www.gradle.org/).  It plugs JavaCC and JJTree directly into the standard Gradle compile system, 
producing bytecode the same way the Java plugin does.

## Installation

Add the following lines to your `build.gradle` script:

    apply plugin:com.smokejumperit.gradle.compiler.JavaccPlugin
    apply plugin:com.smokejumperit.gradle.compiler.JJTreePlugin

    buildscript {
      repositories {
				add(new org.apache.ivy.plugins.resolver.URLResolver()) {
					name = 'GitHub'
					addArtifactPattern 'http://cloud.github.com/downloads/[organisation]/[module]/[module]-[revision].[ext]'
					addIvyPattern 'http://cloud.github.com/downloads/[organisation]/[module]/[module]-[revision].pom'
					m2compatible = true
				}
				mavenCentral()
      }
      dependencies {
        classpath 'com.smokejumperit.gradle.compiler:javacc:0.0.4'
      }
    }

## Usage

Place your JavaCC code into `./src/main/javacc`. If you have source 
sets other than `main` (if you don't know what that means, you don't), then replace `main` with the name of the source set.
The generated code plus any other Java files will end up in `./build/javacc-gen/main`, and will be compiled as part of the Java compile.

To use JJTree, place JJTree code into `./src/main/jjtree`, and it is generated into `./build/jjtree-gen/main`, and compiled with the 
normal JavaCC compile. Due to the magic of transitivity, the generated JavaCC code is itself generated and compiled as part of the Java
compile.

Java files beside (or beneath) JavaCC/JJTree files, and JavaCC files beside (or beneath) JJTree files are also processed. This is 
intended as a convenience for those who were using the JavaCC plugin.

## Changelog

### 0.0.3

In the 0.0.1 version, running `gradle classes; gradle classes` would fail because the up-to-date `compileJavacc` task prevented the
source for `compileJava` from being updated. The 0.0.2 version attempted to fix this, but that fix failed with customized source directories
(the source for `compileJava` was updated before the source directory customization was applied). This fixes it the right way, if in a bit of
overkill.  Also, added this changelog.
