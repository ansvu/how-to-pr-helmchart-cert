Table of Contents
=================

* [Table of Contents](#table-of-contents)
* [How To PR For Partner Helmchart Certification](#how-to-pr-for-partner-helmchart-certification)
   * [Pre-requisite](#pre-requisite)
   * [Fork openshift-helm-charts/charts](#fork-openshift-helm-chartscharts)
   * [Clone Main Charts Repo](#clone-main-charts-repo)
   * [Add Upstream to Main Charts Repository](#add-upstream-to-main-charts-repository)
   * [Add Origin to your own helm-chart that fork from main](#add-origin-to-your-own-helm-chart-that-fork-from-main)
   * [Check Git Remote status for origin and upstream](#check-git-remote-status-for-origin-and-upstream)
   * [Checkout charts Repository as new branch](#checkout-charts-repository-as-new-branch)
   * [Check Git Status](#check-git-status)
   * [Make Directory and Copy Report File](#make-directory-and-copy-report-file)
   * [Git Add, Commit and Push to origin](#git-add-commit-and-push-to-origin)
   * [Start PR from github in your own fork](#start-pr-from-github-in-your-own-fork)
* [Post PR Checking](#post-pr-checking)
* [Git Rebase When OWNERS File Updated After PR](#git-rebase-when-owners-file-updated-after-pr)
* [Ready To Publish Helm Chart](#ready-to-publish-helm-chart)
* [RedHat Certification Chart Verifier Links](#redhat-certification-chart-verifier-links)

   
# How To PR For Partner Helmchart Certification
How to prepare and do PR for Helm Chart Certification

## Pre-requisite
- Helm Chart Project is created from connect.redhat.com
- A report.yaml with all test cases must be passed  
 `when run chart-verifier to generate a report.yaml, it must include -W option (webCatalogOnly)`  
```shellSession
./chart-verifier verify --config config.yaml /var/home/core/deployment/CMM_34_QA/helm_chart/CMM-operator-k8s-23.5.0-p1.tgz --chart-values ~/deployment/CMM_34_QA/system/values.yaml_cert -W
```  
**Note:** Make sure this parameter `webCatalogOnly: true` on report.yaml  
Also, there is new change where this parameter `providerDelivery: true` always present and already defined in OWNERS. 

- OWNERS file is generated with all the correct information after filled out project profile 
---
Example of a good OWNERS file,
```yaml
chart:
  name: cmm-operator-k8s
  shortDescription: unknown
providerDelivery: true
publicPgpKey: unknown
users:
- githubUsername: xxxx
vendor:
  label: nokia
  name: Nokia
```
---
**Note**: The github user: `xxxx`is used as an example, so this user will be your github username  

## Fork openshift-helm-charts/charts
From browser https://github.com/openshift-helm-charts/charts then click fork from top-right  
![Fork Openshift Helm Chart](img/fork-chart.png "Fork Openshift Helm Chart")

## Clone Main Charts Repo
- **From Linux Terminal**
```diff
+ git clone https://github.com/openshift-helm-charts/charts.git
```
## Add Upstream to Main Charts Repository
```diff
+ git remote add upstream https://github.com/openshift-helm-charts/charts
```

## Add Origin to your own helm-chart that fork from main
```diff
+ git remote add origin https://github.com/xxxx/charts.git
```
**Note**: If it says remote origin is already existed, but not under your github user(xxxx) then remove it and re-add with your github-username  
  
To remove remote origin:
```diff
+ git remote remove origin
```
---
## Check Git Remote status for origin and upstream
```diff
+ git remote -v
origin  https://github.com/xxxx/charts.git (fetch)
origin  https://github.com/xxxx/charts.git (push)
upstream        https://github.com/openshift-helm-charts/charts (fetch)
upstream        https://github.com/openshift-helm-charts/charts (push)
```
---
## Checkout charts Repository as new branch
```diff
+ git checkout -b cmm23.5
Switched to a new branch 'cmm23.5'
```
## Check Git Status
```diff
+ git status
On branch cmm23.5
nothing to commit, working tree clean
```
## Make Directory and Copy Report File
- **Partner Project Directory Structure**
```diff
+ cd ./charts/charts/partners/
+ tree nokia
nokia
└── cmm-operator-k8s
    └── OWNERS
```
- **Example of Chart.yaml**
```yaml
annotations:
  charts.openshift.io/archs: x86_64
  charts.openshift.io/name: Nokia CMM
  charts.openshift.io/provider: Nokia
  charts.openshift.io/supportURL: https://github.com/nokiacmm/helm-chart
apiVersion: v2
description: A Helm chart for CMM in Kubernetes using operators
icon: https://www.nokia.com/chart-icon.png
name: cmm-operator-k8s
type: application
version: 23.5.0-p1
kubeVersion: '>= 1.20.0-0'
```
- **Make Directory using helm-chart version**
  Helm-chart version can be found from Chart.yaml or report.yaml
  
  **Note**: `version:` and `name:` must be all lower-case! 

```diff
+ cd nokia/cmm-operator-k8s
+ mkdir 23.5.0-p1
```
- **Copy report file to new directory**
```diff
+ cp report.yaml 23.5.0-p1/
```
---
## Git Add, Commit and Push to origin
```diff
+ git add .
+ git commit -m "Added report.yaml"
+ git push origin cmm23.5
```
---
## Start PR from github in your own fork
From browser https://github.com/xxxx/charts, click on Pull Request, then click on 'Compare & pull request'
![Compare-pull-request](img/final-pr-merge.png "Compare & Pull-Request")  

![Start Final PR](img/pull-request1.png "Start do Helm Chart Final PR")
`Left Base is from main charts repo and branch as 'main', and right base is own-charts(forked) and select cmm23.5 as branch. Normally it should be automatic select correct main base and your own branch`.  
Please just do double-checking it before Click on **'Create Pull Request'** button.  

# Post PR Checking
After Click on Create Pull Reques Button, then go here to check status  
https://github.com/openshift-helm-charts/charts/actions

![PR-Build-Status](img/pr-success-build.png "Check PR Build Status")

# Git Rebase When OWNERS File Updated After PR
- **From Your Local Branch Directory**
```shellSession
git pull upstream main --rebase
git push origin cmm23.5 -f
```

# Ready To Publish Helm Chart
When PR is succesful Merged to main chart repository, then go back to connect.redhat.com and under helm chart project product-list, click on your helm-chart project then click Publish the helm-chart. 
- **Click On Your Helm Chart Project** 
![Helm-Chart-Project](img/helm-chart-project.png "Click on Helm Chart Project")
- **Ready to Publish Helm-Chart**  
![Publish-Helm-Chart](img/click-publish-helm-chart.png "Publish Helm-Chart")
  
Example of real CMM23.5 PR [Click Here](https://github.com/openshift-helm-charts/charts/pull/877)

**Note:** If Product-listing is already set to `Publish`, then once PR is merged it will automatic auto-publish to the catalog.

# RedHat Certification Chart Verifier Links
- RedHat-Certification-Chart-Verifier  
  https://github.com/redhat-certification/chart-verifier
- Helm-Chart-Submission   
  https://github.com/redhat-certification/chart-verifier/blob/main/docs/helm-chart-submission.md
- Helm-Chart-Annotations  
  https://github.com/redhat-certification/chart-verifier/blob/main/docs/helm-chart-annotations.md
- Helm-Chart-Troubeshooting  
  https://github.com/redhat-certification/chart-verifier/blob/main/docs/helm-chart-troubleshooting.md
- Helm-Docs  
  https://github.com/redhat-certification/chart-verifier/blob/main/docs
  
