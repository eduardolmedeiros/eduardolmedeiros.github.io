---
layout: post
title: 'Azure blob storage: azcopy tips'
date: '2019-12-10 11:50:18'
categories: azcopy storage azure 
---

Some utils commands for managing files on Azure Blob Storage.

### copy a single file

{% highlight shell %}
azcopy cp --put-md5 '[source_file]' 'https://[storage_account_name].blob.core.windows.net/[container_name]/[sas_token]'
{% endhighlight %}

### copy recursive files

{% highlight shell %}
azcopy cp --put-md5 --recursive '[source_dir]' 'https://[storage_account_name].blob.core.windows.net/[container_name]/[sas_token]'
{% endhighlight %}

### sync files

{% highlight shell %}
azcopy sync --put-md5 '[source_dir]' 'https://[storage_account_name].blob.core.windows.net/[container_name]/[sas_token]'
{% endhighlight %}

### download single file

{% highlight shell %}
azcopy cp '[source_dir]' 'https://[storage_account_name].blob.core.windows.net/[container_name]/[file]/[sas_token]'
{% endhighlight %}

### delete single file

{% highlight shell %}
azcopy rm 'https://[storage_account_name].blob.core.windows.net/[container_name]/[file]/[sas_token]'
{% endhighlight %}

### delete recursive files

{% highlight shell %}
azcopy rm --recursive=true 'https://[storage_account_name].blob.core.windows.net/[container_name]/[file_or_dir]/[sas_token]'
{% endhighlight %}
