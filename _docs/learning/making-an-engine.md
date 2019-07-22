---
title: Making An Engine
permalink: /docs/making-an-engine/
---

#### An Introduction to Java Game Engine Development

Interested in making a game engine in Java? Well, you've come to the right place. Included here are the basics (at least the basics that *I* followed) that you should familiarize yourself with before embarking on the journey of game engine development. This is by no means an exhaustive list of every single thing you might possibly want to know, but rather everything that you need to get up and running on your own. Going forward, I'll also try to write a bit on some more specific, advanced topics applicable to this engine. Note that, because this is a *Java* game engine after all, a [thorough understanding](https://docs.oracle.com/javase/tutorial/index.html) of the Java programming language is expected.

## So, What Do I Need to Learn?

### Tools

Well, to get started, you should first familiarize yourself with the
technologies and tools you'll be using to program and build your game engine
code. For most avid programmers, you probably already have a pretty clear idea
of what you'll be using, and this is one area that can vary widely between
different people. However, because this guide *is* intended for most people to
pick up and learn from, I'll cover the tools that I chose to use for OpenJGE2D.

#### Maven
A useful tool for building Java Projects.
- [Getting Started Guide](https://maven.apache.org/guides/getting-started/index.html) - Everything you need to know to get up to speed with Maven
- [Standard Directory Layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html) - Maven project structure conventions to follow
- [Maven Overview](https://www.cloudrepo.io/articles/what-is-a-maven-repository.html) - An article providing a brief overview of Maven and Maven repositories
- [Maven Installation](https://www.mkyong.com/maven/how-to-install-maven-in-windows/) - Short Maven installation tutorial. Note that installing Maven is not necessary for building projects in IntelliJ IDEA. However, by installing it, we can run Maven commands from the command line or Git Bash

#### IntelliJ IDEA
My personal favourite Java IDE.
- [Discover IntelliJ IDEA](https://www.jetbrains.com/help/idea/discover-intellij-idea.html?section=Windows) - Navigating the IDE
- [Unit Testing](https://www.youtube.com/watch?v=QDFI19lj4OM) - A short video depicting how to create and run unit tests in IntelliJ
- [Unit Testing Guide](https://www.jetbrains.com/help/idea/configuring-testing-libraries.html?section=Windows) - See all articles under the **Testing** section

#### Git & GitHub
Two of the most popular version control systems.
- [Getting Started with Git](https://medium.com/@prithajnath/getting-started-with-git-7aae82dd3599) - A brief introductory article
- [Getting Started with Git (Again)](https://www.taniarascia.com/getting-started-with-git/) - Another brief introductory article
- [Official Git Book](https://git-scm.com/book/en/v2) - Understanding the first two chapters is a MUST for knowing how to properly use git
- [Official GitHub Guides](https://help.github.com/en#dotcom) - An extensive list of guides for using GitHub
- [Git & GitHub in Practice](https://gist.github.com/bgun/c7447ab0906517221b6b) - A practical example of using IntelliJ with Git & GitHub from the command line
- [gitignore.io](https://www.gitignore.io/) - Useful online tool for generating .gitignore files tailored to your project

#### Documentation
This shouldn’t apply to most people, but I’m including it in the off chance that there are those interested in making an open source engine of their own and want to write documentation for it.
- [Javadocs Guide](https://www.oracle.com/technetwork/articles/javase/index-137868.html) - A guide for writing coherent API documentation as specified by Oracle

### Libraries

While the tools outlined above are subject to change based on the needs and preferences of individuals, the libraries used, on the other hand, should remain mostly consistent amongst developers looking to make a Java game engine. I say mostly though because LWJGL provides access to a wide range of external libraries that can be customized based upon the needs of individuals and the engines they want to create. However, the key technologies (namely GLFW and OpenGL) shouldn't change. Of course, there is more to the story than just GLFW and OpenGL when it comes to designing an entire game engine. However, I'll be honest with you, I haven't quite reached that point just yet myself. So, for the time being I'm just including these three frameworks, but do expect more to appear as development progresses.

#### LWJGL
There isn’t much to learn when it comes to the LWJGL framework, as it provides direct Java bindings to the GLFW and OpenGL APIs, which are written in C++. Despite this, LWJGL is what allows us to make a *Java* game engine as opposed to a C++ game engine, and is thus very important.
- [LWJGL Website](https://www.lwjgl.org/) - The official website of LWJGL. Not much to see here, but this is where you can access the Maven dependencies of the library to put into your project’s POM
- [LWJGL Book](https://ahbejarano.gitbook.io/lwjglgamedev/) - While most of what is in this book is covered in the GLFW and OpenGL sections, it has the benefit of including examples in Java rather than C++. You shouldn’t need to use this book most of the time, as the other two resources are much better for learning GLFW and OpenGL, but it may be useful to follow when you’re just starting out and looking to get the fundamentals set up in your engine.

#### GLFW
GLFW is an easy to use library that provides access to window, OpenGL and input system contexts.
- [GLFW Guides](https://www.glfw.org/docs/latest/) - A series of not-too-long guides covering all aspects of the GLFW library

#### OpenGL
The OpenGL library forms the rendering backbone of most modern game engines.
- [Learn OpenGL](https://learnopengl.com/) - Provides comprehensive lessons on all aspects of OpenGL; a must-have for learning how to write your own rendering engine. Note that all the code examples are written in C++, however, they are quite simple and shouldn’t be too hard to follow

### Other Resources

These resources, although not necessary for building a game engine, come in handy when designing an engine that is easily maintainable and flexible enough to suit a range of different functions and implementations.

#### Game/Engine Design
Learning to program is one thing, but learning to program well - now that’s a whole different story.
- [Game Programming Patterns](http://gameprogrammingpatterns.com/) - The quintessential book for all things engine design! Covers a wide array of useful patterns, and best of all: it’s free
