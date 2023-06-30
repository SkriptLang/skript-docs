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

