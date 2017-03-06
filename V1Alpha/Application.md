## V1Alpha/Application

Application is the top level resource on omniql

example of application definition:

```yaml
metadata:
  name: example
spec:
    reverseHost: io.omniql.example.*
```   

- reverseHost is the unique global  identifier of the application
- after an application is created, no other application  can take the same reverse host name, the older app should be delete first
- delete an application is a danger operation *user should double check* also receive  a  warning about the component that will suffer with this change 
- application names can change at any time 

## schemas 

```yaml
reverseHost
   type: string
    pattern: receive any valid reverse domain name and should en with .*
name
    type: string
```

## limitations:

domain names should always end with a wildcard `.*`, this means that the application will owns  all the top level subdomains of that host,
if the user want to host many application on x subdomain it should declared the application in the subdomain level required 


### example:

you want to host example-0 and example-1 on example.omniql.io, your  app definitions should looks like


```yaml
metadata:
  name: example
spec:
    reverseHost: io.omniql.example.example-0.*
```   

```yaml
metadata:
  name: example
spec:
    reverseHost: io.omniql.example.example-1.*
```   
