# Documentation automation projects

This page describes some documentation automation projects, which were mostly created with Python.

## First automated steps with AWK

My first documentation automation projects were to process code or other text files to create reference documentation.
I used a basic shell script framework and added AWK commands.
For example, I created a table of Java errors in [AWK to process API Error file](https://github.com/mjsmyth/abiquo-wiki-scripts/blob/master/process_api_error/process_api_error_dev_guide.sh).
Now for this process I use a Python script that reads the API errors generated in the build.
The API errors are available on the Abiquo wiki.

## Next steps in developer collaboration

To ensure complete documentation of important system properties, the developers commit to a central properties file. 
The scripts then process the file into a a table for the Abiquo wiki.
The first script was [AWK to process properties file](https://github.com/mjsmyth/abiquo-wiki-scripts/blob/master/properties/process_properties.sh).
The latest version is in Python and it manages multiple variants of single properties.

## Testing a customer use case in Python

A common customer use case is to create a tenant in the cloud platform. 
The customer will then automate this action with the API for their specific environment. 
I created a very quick Python script to [test the steps in the customer use case](https://github.com/mjsmyth/abiquo-wiki-scripts/blob/master/customer_script/abiquotenant.py).
I later also created a wiki page to describe the steps: [how-to create a tenant via API](https://abiquo.atlassian.net/wiki/spaces/doc/pages/311375969/How+to+create+a+tenant+via+API).


## Python to implement API how-tos

When I write an API how-to, I think it's sometimes helpful to check the process with code. 
I enjoyed using Python to quickly implment 
[How to add a NAT rule via API](https://abiquo.atlassian.net/wiki/spaces/doc/pages/311375630/How+to+add+a+NAT+rule+to+a+VM+via+API). 
The Python script is very basic, because it is just to test the process. 
See [Add a NAT rule to a VM with Python](https://github.com/mjsmyth/abiquo-wiki-scripts/blob/master/api_howtos/vm_add_nat_rules.py).

## Check API docs for missing data

I had an HTML version of the API docs and I used Python to scan it for missing data. 
In some cases, the privileges required to use a request were not appearing in the API docs. 
Rather than check 100 pages containing 700 methods by hand, I wrote a script. 
See [Check roles or privileges in API docs](https://github.com/mjsmyth/abiquo-wiki-scripts/blob/master/checkroles/Checkroles.py).


## Migrating to Confluence Cloud

Migrating from Confluence Server to Confluence Cloud took several months. When you get to Cloud, your pages will be in the "legacy editor". 
Hopefully, you will be able to easily convert them to the "new editor" and it will offer all of the functionality necessary for your documentation use case. 
As part of a migration, I wrote scripts to automate many tasks, such as, [check which editor the page is using](https://github.com/mjsmyth/abiquo-wiki-scripts/blob/master/confluence_cloud_migration/ccloGetEditor.py).


## Add data for screenshots using the product API

There were some new UI screens that displayed custom metrics. 
However, custom metrics are metrics that users have added with the API or a tool such as collectd running on the VM. 
To quickly get some screenshots with data, I wrote a very rough script to [push custom metrics](https://github.com/mjsmyth/abiquo-wiki-scripts/blob/master/general/metrics_push.py).
