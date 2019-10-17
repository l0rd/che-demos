# Instructions

## Start minikube

```bash
mk --v=7
```

## Start Che

```bash
./bin/run server:start
kubens che
export EDITOR=vim
k edit cm/che # add CHE_WORKSPACE_SIDECAR_IMAGE__PULL__POLICY: "IfNotPresent"
```

## Deploy a NodeJS / mongoDB application

```bash
cd che-demos/devfile
vim ./deploy_k8s.yaml # replace ingress ip with minikube ip
kubectl create namespace prod
kubens prod
kubectl apply -f ./deploy_k8s.yaml
```

## Generate a devfile

```bash
chectl devfile:generate \
    --git-repo='https://github.com/sleshchenko/NodeJS-Sample-App.git' \
    --language=typescript
```

```bash
chectl devfile:generate \
    --git-repo='https://github.com/sleshchenko/NodeJS-Sample-App.git' \
    --language=typescript \
    --namespace='prod' \
    --selector='app.kubernetes.io/name=employee-manager'
```

## Edit the devfile

1. make k8s components alias simpler
2. override nodjs app entrypoing to `tail -f /dev/null`
3. Add commands to start and stop the application

## Start a Che workspace

```bash
chectl workspace:start --devfile=ready-to-use.devfile.yaml
```

---

## Problems

- containers list is not loaded
- if, after port plugin have found/associated the nodjs port, I refresh (CMD + R) in Theia then I get a 502 bad gateway error
- Clicking on a stack trace opens a tab with an error
- When selecting start task I am asked which container (it should be automated)
- When stopping nodjs I would expect the run tab to get closed
- When stopping task is completed I would expect the tab to be closed (or at least inform me about the command has terminated)
- I like opening application in a new tab instead of preview
- No yaml syntax highlighting
- che-plugin-registry default image should be quay not docker hub
- copy/paste of a command doesn't work

---

## New script

- Short Che intro: Che is a container based IDE...simplify dev...dev/prod parity
- Show a simple devfile and the corresponding workspace (a github repo and a language)
- Show a kube app with a problem
- Show how we can generate a devfile that includes a kube app
- Repro the bug
- Fix the bug