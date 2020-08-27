## What is an artifact?﻿

An artifact is an assembly of your project assets that you put together to test, deploy or distribute your software solution or its part. Examples are a collection of compiled Java classes or a Java application packaged in a Java archive, a Web application as a directory structure or a Web application archive, etc.

An artifact can be an archive file or a directory structure that includes the following structural elements:

- Compilation output for one or more of your modules
- Libraries included in module dependencies
- Collections of resources (web pages, images, descriptor files, etc.)
- Other artifacts
- Individual files, directories and archives.

## [What is a Maven artifact?](https://stackoverflow.com/questions/2487485/what-is-a-maven-artifact)

An artifact is a file, usually a JAR, that gets deployed to a Maven repository.

A Maven build produces one or more artifacts, such as a compiled JAR and a "sources" JAR.

Each artifact has a group ID (usually a reversed domain name, like com.example.foo), an artifact ID (just a name), and a version string. The three together uniquely identify the artifact.

A project's dependencies are specified as artifacts.