---
layout: post
title: Setting up Android application to use Proguard
categories: []
tags: []
status: publish
type: post
published: true
meta: {}
---

ProGuard is a free Java class file shrinker, optimizer, obfuscator, and preverifier. It ships with the android sdk.

Proguard is an  essential tool while getting an Android app ready for release. From a developer's perspective, proguard offers three benefits

* Compresses code and makes the application smaller. Results can be seen <a href="http://proguard.sourceforge.net/#results.html">here</a>.
* Obfuscates code so that it is slightly harder for someone to understand your code.
* Can automatically remove Log statements and other debugging code.

If you have not already added ant builds to your project do so by

	$>android update project -p .

When an Android project is created, a proguard-project.txt and project.properties are automatically created in the project's folder.  
To enable proguard uncomment the following line in project.properties
	#proguard.config=${sdk.dir}/tools/proguard/proguard-android.txt:proguard-project.txt
If you would like to turn on optimization( as is required to remove Logging), change the filename to proguard-android-optimize.txt.
	proguard.config=${sdk.dir}/tools/proguard/proguard-android-optimize.txt:proguard-project.txt

proguard-project.txt contains all the arguments that are required to be passed to proguard. If you run `ant clean release` now, your APK'
should be optimized. 

Run your application on a device and test all functionality. You might need to play with the -keep argument to end up with an apk that 
has all the required classes included.

Note
-----
If you have included the android support library, you might need to add the following to the proguard-project.txt 
	-keep class android.support.** { *; } 
	-keep interface android.support.** { *; } 

If you are using SimpleXML , the following might help
	-keepattributes *Annotation*
	-keepattributes Signature, *Annotation*
	-keep public class org.simpleframework.**{ *; } 
	-keep class org.simpleframework.xml.**{ *; } 
	-keep class org.simpleframework.xml.core.**{ *; } 
	-keep class org.simpleframework.xml.util.**{ *; }