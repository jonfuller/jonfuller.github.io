---
layout: post
title: Downloading a JAR's Dependencies
categories:
- blog
date: 2014-05-22 15:00
---

Ever find a `jar` that has it's dependencies declared in a `pom`, but you don't use [maven](http://maven.apache.org/)?

I do all the time... because I don't __ever__ use maven!

So, here's the deal:

1. create a `pom` that looks something like this:

        <?xml version="1.0"?>
        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
          <modelVersion>4.0.0</modelVersion>
            <groupId>com.doesnt.matter</groupId>
            <artifactId>AlsoDoesntMatter</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>Simple POM to download some dependencies</name>
            <url>http://jonfuller.co</url>
            <dependencies>
                <dependency>
                    <groupId>com.netflix.rxjava</groupId>
                    <artifactId>rxjava-core</artifactId>
                    <version>0.18.3</version>
                    <scope/>
                </dependency>
            </dependencies>
        </project>

    The example above downloads the dependencies for Netflix's `rxjava-core` (version 0.18.3) jar.

    __Note__: Figure out your dependeny's actual `groupId`, `artifactId`, and `version` numbers are by checking out [maven central](http://search.maven.org/)

1. execute this at your terminal:

        mvn -f example-pom.xml dependency:copy-dependencies

    __Need maven?__  `brew install maven`, if you're on OS X.

1. wait while maven downloads the Internet.

1. bask in the glory of your new pile of jars. (located at `target/dependency`)


For me, I was really wanting to download the dependencies for [retrofit](http://square.github.io/retrofit/).  I've dropped the above and an example for retrofit into a gist for your viewing pleasure:

* [Download POM for RxJava](https://gist.github.com/jonfuller/346514743cfa79cbc447#file-rxjava-core-pom-xml)
* [Download POM for RetroFit](https://gist.github.com/jonfuller/346514743cfa79cbc447#file-retrofit-pom-xml)
