# kustomize-issue
files needed for opening a kustomize issue


The issue:

This was tested with both kustomize version 3.2.1 and 3.5.4.
All our infra is built with kustomize, so anything you read below was created with kustomize, nothing out of its scope.

Scenario: 
Until now, we had only one nginx-controller. This was ok and working well. Then, we had the need to create another nginx controller. For that, in order to avoid duplication of code, we created a dynamic-nginx-controller folder which consists of resources that supports variables - deployment name is nginx-deploy-$(APP), service name is service-$(APP) etc. This works when we have one controller. BUT, under base, when we reference both nginx controllers, and I try to run kustomize build while i'm in dev folder, that merges base resources with patches of it's own, it fails on-
 
Error: accumulating resources: recursed accumulation of path '../../../base': accumulating resources: recursed merging from path 'nginx-controllers/stam': may not add resource with an already registered id: apps_v1_Deployment|nginx-ingress-$(APP)|nginx-ingress-$(APP)

Trying to paste the configuration here creates a mess, so I uploaded it as a repo in my GitHub account. You can view it here-

How to test:
* one app only - works:
1. cd kustomize-issue/dev/us-central1/apps
2. kustomize build .
output: kustomize builds all resources ok, with the app variable value populated

* two apps, a.k.a merging resources - not working:
1. remove remark from "# - nginx-controllers/app2 " in kustomize-issue/base/kustomization.yaml
2. cd kustomize-issue/dev/us-central1/apps
3. kustomize build .
output: Error: accumulating resources: recursed accumulation of path '../../../base': accumulating resources: recursed merging from path 'nginx-controllers/app2': may not add resource with an already registered id: apps_v1_Deployment|nginx-ingress-$(APP)|nginx-ingress-$(APP)

Please advise. We already have a need a third nginx-controller, and we really want to avoid duplication of code.
