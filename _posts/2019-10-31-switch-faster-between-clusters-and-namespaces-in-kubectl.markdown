---
layout: post
title: 'k8s: Switch faster between clusters and namespaces in kubectl'
date: '2019-10-31 15:31:41'
categories: k8s kubernetes
---

Managing cluster and namespace in k8s sometime is quite complicated especially when you have to deal with multiples resources.

There is a great project called [kubectx](https://github.com/ahmetb/kubectx) that makes your life easy.

**kubectx** helps you switch between clusters back and forth and **kubens** helps you switch between Kubernetes namespaces smoothly.

{% highlight shell %}
$ kubectx minikube
Switched to context "minikube".

$ kubectx -
Switched to context "oregon".

$ kubectx -
Switched to context "minikube".

$ kubectx dublin=gke_ahmetb_europe-west1-b_dublin
Context "dublin" set.
Aliased "gke_ahmetb_europe-west1-b_dublin" as "dublin".
{% endhighlight %}


Serious, this is such great tool!  
  
Have a look at [github project.](https://github.com/ahmetb/kubectx)

