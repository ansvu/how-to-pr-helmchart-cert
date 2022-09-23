# how-to-pr-helmchart-cert
How to prepare and do PR for Helm Chart Certification

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
+ git remote add origin https://github.com/ansvu/charts.git
```

## Check Git Remote status for origin and upstream
```diff
+ git remote -v
origin  https://github.com/ansvu/charts.git (fetch)
origin  https://github.com/ansvu/charts.git (push)
upstream        https://github.com/openshift-helm-charts/charts (fetch)
upstream        https://github.com/openshift-helm-charts/charts (push)
```
## Checkout charts Repository as new branch
```diff
+ git checkout -b cmm22.5
Switched to a new branch 'cmm22.5'
```
## Check Git Status
```diff
+ git status
On branch cmm22.5
nothing to commit, working tree clean
```
---
## Make Directory and Add Report File
- **Partner Project Directory Structure**
```diff
+ cd ./charts/charts/partners/
+ tree nokia
nokia
└── cmm-operator-k8s
    └── OWNERS
-----------------------------
+ cd nokia/cmm-operator-k8s
----------------------------
```
- **Make Directory using helm-chart version**
  Helm-chart version can be found from report.yaml
  
  **Note**: version: 22.5.0-P4 --> P4 should be lower case p4. If there are CAPs, then UPPER-->lower

```diff
+ mkdir 22.5.0-p4
```
- **Copy report file to new directory**
```diff
+ cp report.yaml 22.5.0-p4/
```
---
---
### Git Add, Commit and Push to origin
```diff
+ git add .
+ git commit -m "Added report.yaml"
+ git push origin cmm22.5
--------------------------------------
**Note**: if git push got error then try -f to force it
+ git push origin cmm22.5 -f
```
---
### Start PR from github in your own fork
From browser https://github.com/ansvu/charts, click on Pull Request, then click on 'Compare & pull request'
![Compare-pull-request](img/final-pr-merge.png "Compare & Pull-Request")  

Left Base is from main charts repo and branch as 'main', and right base is own-charts(forked) and select cmm22.5 as branch  
Normally it should be automatic select correct main base and your own branch but just do double-checking it before Click on **'Create Pull Request'** button.  
![Start Final PR](img/pull-request1.png "Start do Helm Chart Final PR")

