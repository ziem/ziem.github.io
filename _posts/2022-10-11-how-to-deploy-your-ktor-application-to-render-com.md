---
layout: post
title: How to deploy your Ktor application to Render?
---

Recently, after Heroku announced removal of their free plans, I migrated my hobby project from Heroku to Render.

Render, <https://render.com/>, is yet another cloud application hosting. According to the Internet, it's a good alternative to Heroku.
It doesn't support Java out of the box, but you can host anything with Docker.

To deploy your Ktor application, we need two things. First, add `io.ktor.plugin` plugin to your root build.gradle file:
```kotlin
id("io.ktor.plugin") version "2.1.2"
```

This plugin adds `buildFatJar` Gradle task which creates an executable JAR with all dependencies.

Next, you create a new file in your root directory named `Dockerfile`. `Dockerfile` is a text file that describes how to create an image. In our case, we use `Dockerfile` from [Ktor documentation](https://ktor.io/docs/docker.html#prepare-docker):
```dockerfile
FROM gradle:7-jdk11 AS build
COPY --chown=gradle:gradle . /home/gradle/src
WORKDIR /home/gradle/src
RUN gradle buildFatJar --no-daemon

FROM openjdk:11
EXPOSE 8080:8080
RUN mkdir /app
COPY --from=build /home/gradle/src/build/libs/*.jar /app/ktor-docker-sample.jar
ENTRYPOINT ["java","-jar","/app/ktor-docker-sample.jar"]
```

Keep in mind to replace the `ktor-docker-sample` with your project name.

Now it's time to do `git commit` & `git push`.

Next, in the <https://render.com/> dashboard, you select New -> Web Service and connect your GitHub/GitLab repo. Default options are good enough.

After that, the build should start, and at the end you should see a green tick with "Deploy live for XXX" text.

Happy building!
