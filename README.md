# DSL Implementation Example


Kotlin DSL scripts - Domain Specific Language

Just like the Groovy-based equivalent, the Kotlin DSL is implemented on top of Gradle’s Java API.

Everything you can read in a Kotlin DSL script is Kotlin code compiled and executed by Gradle.
Many of the objects, functions and properties you use in your build scripts come from the Gradle API and the APIs of the applied plugins.

Note :-

1. Groovy DSL script files use the .gradle file name extension.

2. Kotlin DSL script files use the .gradle.kts file name extension.

Steps :-

To activate the Kotlin DSL, simply use the .gradle.kts extension for your build scripts in place of .gradle.
That also applies to the settings file —
for example settings.gradle.kts — and initialization scripts.


DSL stands for Domain Specefic Language, which is a type of programming language that cares mainly about the readability by enforcing the use of declarative code with minimal boilerprate (declare to use) instead of imperative code (explaining how to solve a problem) but as a drawback,
it can be used only inside a specific context or domain.

Kotlin is already proven to be a very powerful and a safe production-ready programming language especially. As a GPL (General Purpose Langage) Kotlin has some great features that enables writing very clean code with minimal syntax, things like Lambdas with receivers that can be used outside of method parentheses and infix function notation.


Shortly, because Groovy is a dynamically typed language, and because it’s very likely that you have come across one of these problems when dealing with build script files for Android :

Kotlin is statically typed language


If Null Pointer Exception is the Billion dollar mistake, build waiting time is easily the Trillion Dollar mistake !


Evaluating promises

A Type-safe DSL — ✓ — Because it’s statically typed, Kotlin DSL provides more safety on passed parameters when invoking Gradle build functions.
Interoperability with existing scripts — ⚠︎—because both are JVM languages (and because Jetbrains has done great job) Kotlin DSL scripts are compatible with Groovy but not fully because of Groovy’s dynamic types. Some Gradle plugins will need to be modified in order to be used in Kotlin DSL
Maximum readability
Consistency and super power — ✓ — Having a declarative type-safe language for build is very promising.


Kotlin is a programming language that is rich with interesting features that can make the code more concise and expressive.

One of them is support for writing domain-specific language or DSL. DSL is a computer language devoted to a particular application domain. It is different from general-purpose language that can be applied in all application domains.


Let’s Migrate from Groovy to Kotlin DSL!
To start it, please create a new project with Android Studio. Don’t forget to choose Kotlin as the language to be used. Here I use Android Studio version 3.4 with Kotlin version 1.3.31. After that, check the Gradle version in the gradle-wrapper.properties file:

Here we will focus on 3 files contained in the project, settings.gradle, build.gradle (project), and build.gradle (app). The three files are Gradle scripts which are written by default with Groovy. Now it’s time for us to migrate from Groovy to Kotlin DSL in these files.
First, change settings.gradle to settings.gradle.kts and make sure the code inside will be error. Why? Because you have changed the Gradle file to Kotlin scripts, of course you have to change the code inside to Kotlin DSL.


// before
include ':app'

// after
include(":app")

At this point we have seen a simple difference from Groovy and Kotlin DSL, the String on Groovy can be quoted with single quotes ‘string’ while Kotlin requires double quotes “string” and parentheses () to carry out their functions.



Last but not least, we need to modify the build.gradle (app) file. After changing it to build.gradle.kts, you will facing errors like the following picture:

//from:
apply plugin: "com.android.application"
apply plugin: "kotlin-android"
apply plugin: "kotlin-android-extensions"

//to:
plugins{
    id("com.android.application")
    kotlin("android")
    kotlin("android.extensions")
}

Change it to something like this:
android {
    compileSdkVersion(28)
    defaultConfig {
        applicationId = "com.nrohmen.kotlinandroidcleanarchitecture"
        minSdkVersion(19)
        targetSdkVersion(28)
        versionCode = 1
        versionName = "1.0"
        testInstrumentationRunner = "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        getByName("release"){
            isMinifyEnabled = false
            proguardFiles(getDefaultProguardFile("proguard-android-optimize.txt"), "proguard-rules.pro")
        }
    }
}

Here you need to pay attention to how a plugin is applied. In Groovy, you can use an equal sign (=) to set a value, with Kotlin DSL you need to know in advance whether the configuration is a function or property.
Inside the buildTypes block we use getByName() because actually the release is a string that is passed to a function called by Groovy and Kotlin will not be able to access it. MinifyEnabled is also not an actual property name, so we need to use the original name, which is isMinifyEnabled.
Then in the block dependencies, we need to adjust the code to be as follows:



dependencies {
    implementation(fileTree(mapOf("dir" to "libs", "include" to listOf("*.jar"))))
    implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.21")
    implementation("com.android.support:appcompat-v7:28.0.0")
    implementation("com.android.support.constraint:constraint-layout:1.1.3")
    testImplementation("junit:junit:4.12")
    androidTestImplementation("com.android.support.test:runner:1.0.2")
    androidTestImplementation("com.android.support.test.espresso:espresso-core:3.0.2")
}

