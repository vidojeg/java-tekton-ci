# java-tekton-ci

Build java gradle app with Kaniko and push to AWS ECR.

## Pipeline requirement

- Kubernetes >= 1.25
- Tekton
- Sealed secrets

## Pipeline Inputs

- ECR REPO
- GIT APP REPO
- APP REVISION

If you want to include ArgoCD for the CD part just insert:

- ARGO REPO
- ARGO REVISION
- LOCAL FILE FOR CHANGES

## Pipeline Tasks

- git-clone
- java-gradle
- ecr-login
- kaniko

## SECRETS

You need a create next secret instead of use Sealed secrets:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: aws-credentials
data:
  credentials:
  config:
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: docker-config
stringData:
  config.json: |-
    { "credsStore": "ecr-login", "credHelpers":{"<AWS_ACC_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com":"ecr-login"}} }
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: git-credentials
data:
  id_rsa:
```
  