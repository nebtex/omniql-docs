# Organization

Organization is the top level resource

- all the other resources are part of an organization
- there can be as many organization as needed

organization can be create at any time, they will be created with a unique global identifier, 



```yaml
apiVersion: v1alpha1
kind: Organization
metadata:
  name: nebtex
spec:
    id: 74544kkm
    domains:
    - nebtex.com
    - omniql.io
    
```