# Addon Tutorial (Setup)

NOTE: Please take effort and time into your addons.
Websites like skUnity will not appreciate cheap quality addons made in a few hours on their site.
Your addon will most likely get rejected if it's of such quality.

Scripts may take a few minutes to hours to create, but addons are suppose to be tested and well processed. So please don't release trash addons. Thank you.

## Table of contents (TODO needs html references when done)
```
Table of contents:
    Getting started (This page)
    Registering your Addon
    Creating an Expression
    Creating an Effect
    Creating a Condition
    Creating an Event
    Creating a Type/ClassInfo
    Creating Enum names (Finishing soon)
    Tips and Tricks (Finishing soon)
```

## Information:

So to begin this tutorial, It's excepted that you have some sort of basic Java understanding. You can google "Java 101" or look at some of these links provided:

- https://www.javaworld.com/category/learn-java/
- https://www.oracle.com/java/index.html or https://docs.oracle.com/javase/tutorial/java/index.html (Valid source)
- https://www.codecademy.com/learn/learn-java/
- http://www.tutorialspoint.com/java/
- https://netbeans.org/kb/articles/learn-java.html

And a well understanding of the Spigot API https://hub.spigotmc.org/

You may also choose to watch an updated Youtube video to help learn visually as well.

This tutorial is not designed to go over Java or the Spigot API.

A good understanding of Skript scripting is recommend aswell.
If you don't know basic Skript scripting, it'll be harder to understand what is passed through your addon syntaxes.

This tutorial was written to target Java 8 because we want our addon in this tutorial to target Spigot 1.7+.
If you want your addon to only support Java 17 or latest, you can only target your addon for Spigot 1.17+ as those versions changed to requiring Java 17+.

Do note when you write your plugin.yml you insert `api-version: 1.13` or whatever version you want to support. Also note this tag was only present for 1.13+.

## Requirements:

An IDE with proper Java support. You can use whatever, but one of the following are recommended:
- Eclipse http://www.eclipse.org/
- Jet brains - IntelliJ IDEA http://www.jetbrains.com/idea/
- .NET Beans https://netbeans.org/
- A Skript jar or Maven/Gradle setup of Skript https://github.com/SkriptLang/Skript/releases (https://repo.skriptlang.org/releases)
- A Spigot jar or Maven/Gradle setup of Spigot http://spigotmc.org/ (Use build tools)
- A Minecraft server to test

Skript was made with Eclipse as recommended and Spigot recommends netbeans. We have since added support for IntelliJ, so the preference doesn't matter.

Eclipse 2023/03 is used in this tutorial. Keep in mind this tutorial may get outdated at some point, and we'll try our best to change critical points.

## Getting started:

In this tutorial we'll be using Gradle as our dependency manager, as that's what Skript uses.
First thing we need to do is create a Gradle project. (You may need to install the Gradle plugin if using an older version. Gradle plugin is installed by default now).

New... > Other > Gradle Project

<img width="375" alt="setup-1" src="https://github.com/SkriptLang/skript-docs/assets/16087552/062c6d04-5af3-4146-ab1f-2c75f35dd297">

<img width="475" alt="setup-2" src="https://github.com/SkriptLang/skript-docs/assets/16087552/817ae63d-39eb-4359-90a0-bcbcc4a432ff">

Then click next until you get to the naming screen, and insert your addon name. Then click Finish with default values. (You can modify other settings yourself). Gradle will then setup a project for you.

<img width="493" alt="setup-3" src="https://github.com/SkriptLang/skript-docs/assets/16087552/6423a483-82b7-4aef-a54c-40c6fa39a9b6">

So now your project should look similar to this image, if it doesn't you can git clone the addon tutorial template which should be present on the SkriptLang organization, or add the missing source folders yourself.

<img width="194" alt="setup-4" src="https://github.com/SkriptLang/skript-docs/assets/16087552/01db0f6a-4995-4053-9a81-4a21d2b5841b">

This has a resources source folder and some classes which this tutorial will go over. We first made a package for our main class.
This tutorial uses `me.example.addontutorial` me is a good starting domain, if you own a legit domain, it's best to use that, otherwise me.* is a valid package naming for projects.
Replace `example` in the domain with your name and `addontutorial` with your addon name.

The first file we need to address before we get into the main class is the build.gradle. This is where we tell the project what libraries our project needs/has access to.
So the following is a basic build.gradle file based off of Skript's own current build.gradle

```groovy
import org.apache.tools.ant.filters.ReplaceTokens

plugins {
	id 'com.github.johnrengelman.shadow' version '8.1.1'
	id 'eclipse'
	id 'java'
}

compileJava.options.encoding = 'Cp1252'
compileTestJava.options.encoding = 'Cp1252'

compileJava {
    sourceCompatibility = '1.8'
    targetCompatibility = '1.8'
}

repositories {
	mavenCentral()

	//Spigot
	maven {
		url 'https://hub.spigotmc.org/nexus/content/repositories/snapshots/'
	}

	// Bungeecord
	maven {
		url "https://oss.sonatype.org/content/repositories/snapshots/"
	}

	// Skript
	maven {
		url 'https://repo.skriptlang.org/releases'
	}

}

dependencies {

	//Nullable annotation
	implementation (group: 'org.eclipse.jdt', name: 'org.eclipse.jdt.annotation', version: '2.2.700')

	//Spigot/Bukkit
	implementation (group: 'org.spigotmc', name: 'spigot-api', version: '1.19.4-R0.1-SNAPSHOT')

	//Skript
	implementation (group: 'com.github.SkriptLang', name: 'Skript', version: '2.7.0-beta3') {
		transitive = false
	}

	// bStats
	shadow (group: 'org.bstats', name: 'bstats-bukkit', version: '3.0.2')

}

processResources {
	filter ReplaceTokens, tokens: ["version": project.property("version")]
	from ("lang/") {
		include '*.lang'
		into 'lang/'
	}
}

shadowJar {
	dependencies {
		include(dependency('org.bstats:bstats-bukkit'))
		include(dependency('org.bstats:bstats-base'))
	}
	relocate 'org.bstats', 'me.example.addontutorial.bstats'
	configurations = [project.configurations.shadow]
	archiveVersion = project.property("version")
	minimize()
}

```

So now let's go over this file.
The `plugins` section is our gradle plugins. In this tutorial we'll be using shadow, eclipse and java itself.
The shadow plugin allows us to incorporate a library inside our plugin. Remember to not shadow in Skript itself.
This tutorial we'll show you how to use bStats which is a metrics system for tracking details of our addon users, like amount of servers using your addon, java version etc.
Helpful information to tailor your addon to your users.

The eclipse addon allows you to build a gradle project around your eclipse IDE. You can open a command prompt, change directory to your project `cd "path/to your/ project"`
Then once in this you can type `gradlew eclipse` and gradle will setup your eclispe project for you. If gradlew does not exist for you, you'll need to install gradle.
Once you have gradle installed you can execute the same command without the `w` as the w stands for wrapper. You can type `gradle wrapper` to install a wrapper into your project.
A wrapper allows you to publish a gradle version with your plugin, so others can use the same wrapper for compatability.
Once you have your build.gradle ready after reading this tutorial page, you can type `gradlew clean build` the `clean` in this command removes the build cache folder. The `clean` is optional but recommended to constantly do with your commands.
The `build` command generates a build in `build/lib/addontutorial.jar` but remember that we use `gradlew clean shadow` which is similar but also includes our shadow dependencies in the jar.

The following;

```groovy
compileJava.options.encoding = 'Cp1252'
compileTestJava.options.encoding = 'Cp1252'
```

Sets our project to use the defined charset. Skript uses `UTF-8` you can set that here, but since it's your addon, 'Cp1252' supports a large set of characters if you want some special characters, up to you.

The following;

```groovy
compileJava {
    sourceCompatibility = '1.8'
    targetCompatibility = '1.8'
}
```

Will set our project to target Java version 8 as stated at the starting of the tutorial. This will ensure all your coding gets formatted down to Java 8 regardless of what Java version your IDE is set as.

The following;

```groovy
repositories {
	mavenCentral()

	// Spigot
	maven {
		url 'https://hub.spigotmc.org/nexus/content/repositories/snapshots/'
	}

	// Bungeecord
	maven {
		url "https://oss.sonatype.org/content/repositories/snapshots/"
	}

	// Skript
	maven {
		url 'https://repo.skriptlang.org/releases'
	}

}
```

Will tell gradle where to look for our libraries. The `mavenCentral()` is a very widely used maven repository containing a large set of public libraries. It's good to have.
And the rest of the following are dependencies we need for our addon. Bungeecord is needed for Spigot's ChatComponents otherwise you may have errors stating missing libraries.

The following;

```groovy
dependencies {

	// Nullable annotation
	implementation (group: 'org.eclipse.jdt', name: 'org.eclipse.jdt.annotation', version: '2.2.700')

	// Spigot/Bukkit
	implementation (group: 'org.spigotmc', name: 'spigot-api', version: '1.20.1-R0.1-SNAPSHOT')

	// Skript
	implementation (group: 'com.github.SkriptLang', name: 'Skript', version: '2.7.0-beta3') {
		transitive = false
	}

	// bStats
	shadow (group: 'org.bstats', name: 'bstats-bukkit', version: '3.0.2')

}
```

MODIFY TO LATEST VERSIONS. THIS MAY BE OUTDATED.
This section defines the exact libraries to search the repositories for. The `Nullable annotation` library is what Skript uses for Nullable annotations, BUT Spigot itself includes IntelliJ's Nullable annotation itself.
The Nullable library does not matter, the IDE only looks for the `@Nullable` string tag, and doesn't matter where it comes from.

`By default Gradle resolves all transitive dependencies specified by the dependency metadata. Sometimes this behavior may not be desirable e.g. if the metadata is incorrect or defines a large graph of transitive dependencies. You can tell Gradle to disable transitive dependency management for a dependency by setting transitive = false` This comes from Gradle's documentation, we do not want Skript to include all it's metadata, as there is a large amount of libraries we don't want.

And lastly we're gonna shadow bstats into our project, keep in mind that we need to relocate that package, and only bstats for shadowing, as bstats doesn't operate on it's default domain `org.bstats` as multiple plugins could use bstats. Metrics is totally optional.

The following;

```groovy
processResources {
	filter ReplaceTokens, tokens: ["version": project.property("version")]
	from ("lang/") {
		include '*.lang'
		into 'lang/'
	}
}
```

Uses the ReplaceTokens that we imported at the top of the build.gradle folder. This allows us to replace `@version@` with our version anywhere in our code, very helpful.
The `lang/` folder is a file we'll go over later in the tutorial, as it's needed when working with enums in Skript, for now all this does is includes it in our jar. You can add other folders here if you wish to include more folders you amy have.

The `project.property("version")` tag grabs the defined version from our project. You can define that in the build.gradle file with a line like `version = '1.0'` or what we like to do is create a gradle.properties folder and insert `version=1.1.0` into this file. Gradle will load all the properties automatically.

The following;

```groovy
shadowJar {
	dependencies {
		include(dependency('org.bstats:bstats-bukkit'))
		include(dependency('org.bstats:bstats-base'))
	}
	relocate 'org.bstats', 'me.example.addontutorial.bstats'
	configurations = [project.configurations.shadow]
	archiveVersion = project.property("version")
	minimize()
}
```

Is how we configure the shadowJar to work. We need to include the bstats libraries, and relocate as stated above, we need to tell it to grab the `shadow` configurations which is part of the shadow plugin, set the archiveVersion to our version, this makes the jar `AddonTutorial-VERSION.jar`.
And lastly the minimize will remove libraries our addon doesn't use, to minimize the jar size as a large jar is not pleasent.

Inside settings.gradle should be only `rootProject.name = 'AddonTutorial'`

Now you're all setup and we can start making our addon like a normal Spigot plugin.
