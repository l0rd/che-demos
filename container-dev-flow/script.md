# Container based application development with Che

## Introduction

## Steps

- Start a Che workspace from a Devfile
- Talk about the project (e.g. kubernetes operator): what it does, show the source code, what k8s resources it uses, how it is deployed etc...
- Show the inner loop: run the operator (that creates a nginx pod, a service and route), do changes, restart, debug...
- Show the outer loop: build the Dockerfile, push to the local registry, use the k8s plugin to update an existing operator that is running in another namespace
- Create a PR and push changes
