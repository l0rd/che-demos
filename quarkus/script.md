# Quarkus Demo

## Introduction
The goal of this demo is to provide a developer environment to get started in coding with Quarkus.

Let's say you would like to start the Quarkus helloworld microservice app tutorial from https://quarkus.io/guides/getting-started-guide and have generated the project.
You would like to setup your environment environment for that project.

- You need `mvn`, `GraalVM` and so on to build the project.
- You need a IDE to have the Java intellisence (code completion)
- You would like to be able to test the devmode
- You would like to perform the native compilation of the app
- You would like to test the native app in the real container that would be used in production.

This [devfile](https://github.com/sunix/che-quarkus-demo/blob/mastehttps://github.com/sunix/che-quarkus-demo/blob/master/devfile.yamlr/devfile.yaml) (to be used with Che) will provide you all the element to generate a working ready to use `devspace` :
- the che-theia IDE with a front end to code
- the project to clone: https://github.com/sunix/che-quarkus-demo/.
- a container for running the devmode, compiling the classic `java` app and compiling the native app
- a JDT LS VScode extension to have the java intellisence
- a container where to run the native app
- all the ready to use commands to perform the previous action on the right containers (devmode, package native, run native)
- a git container (to be accessed from the terminal) in case you would like to commit/push your changes somewhere.

## Setup
1. Create minishift/minikube VM
    - Demo is tested with minikube VM with 10GB memory allocated
    - Started on linux VM with the commnand:
       ```
       minikube start --vm-driver=kvm2 --extra-config=apiserver.authorization-mode=RBAC --cpus 4 --memory 10240 --docker-opt userland-proxy=false
       ```

2. Deploy Che Server to local cluster
    - Downloaded the `chectl` binary from https://github.com/che-incubator/chectl/releases and moved/renamed to your `PATH`
        ```
        chectl server:start
        ```

3. Download the devfile from https://github.com/sunix/che-quarkus-demo/blob/master/devfile.yaml locally
   (either by cloning or downloading just the raw file)

4. Create a workspace from this Devfile:

    ```
    chectl workspace:start --devfile=devfile.yaml
    ```

5. Run the workspace.


The devfile will clone this project https://github.com/sunix/che-quarkus-demo.

It is a simple helloworld quarkus project created from the Quarkus getting started
guide https://quarkus.io/guides/getting-started-guide.

The devfile will also setup few container images:
- `quarkus-builder`: based on [quay.io/quarkus/centos-quarkus-maven](https://quay.io/repository/quarkus/centos-quarkus-maven?tag=latest&tab=tags).
  It will provides all the dependencies to run quarkus in devmode and package a quarkus app:
    1. Start the quarkus app in devmode. With the right command `compile quarkus:dev`.
    2. Running it should popup windows to access to the exposed port of the app.
    3. Open the link into another tab and access to the `/hello`. It should display `hello`.
    4. Make a change on the Java file `sunix-quarkus-demo/src/main/java/org/sunix/QuarkusDemoResource.java`,
       use the code completion to perform a `"hello".toUpperCase();`. BEWARE, autosave may not be activated by default (File > Auto Save)
    5. Don't forget to update the test `sunix-quarkus-demo/src/test/java/org/sunix/QuarkusDemoResourceTest.java`
    6. Refreshing the app tab page, it should display `HELLO`
    7. Stop it : `pkill java`
    8. Package the app into a classic java app (optional)
    9. Package the app into a tiny executable by running the native compilation `package -Pnative`, it will take a while ....
- `quarkus-runner`: which is based on `registry.fedoraproject.org/fedora-minimal`
    1. It is used to run the result of the native compilation. Show that it is very tiny `16M` !!!
    2. Once native compilation is perform you can run the command `start native`

In quarkus, we want to run the classic build with a hotspot JVM for developping.
But at some point, we also would like to be able to perform the native compilation
and run the app like it would be in production .... BEFORE PUSHING TO GIT !!!!

This devfile is providing a environment to code the Quarkus app hello world that could be used out of the box.
Developers won't have to install the right JVM and GraalVM and change their default settings.

You can talk about the fact that this environment is very close to the production but also to your CI and take the example of a CI build based on the multistage docker https://github.com/sunix/che-quarkus-demo/blob/master/Dockerfile. and https://quay.io/repository/sunix/quarkus-demo-app?tab=builds

Latest slide deck: https://docs.google.com/presentation/d/12C1TR104PPRftTrzYEn9anI_VOkEYLowMlQCzRJsVYA/edit?usp=sharing
