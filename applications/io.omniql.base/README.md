# io.omniql.base.v1alpha

this app contains the omniql predeclared identifiers, the basic building blocks of any any omniql schema.

# spec

- base should not depends of any other component(databases, pub/sub system, etc) nor any other application
- is expected that definitions never change at all
- due to the nebtex plan to use flatbuffers extensively, they are first citizen on omniql, this means
  that if we found incompatibilities with other schemas, flatbuffers always will be favored 
- for simplify the initial design spec, defaults will not be supported temporally, 
  this is intended to be addressed by type extensions and build pipelines

### versioning 

  - base does not support automatic versioning, so people should be careful changing this spec (in a perfect world the base app schema should never change) 

## Built-in scalar types are (same as flatbuffers):

**8 bit**: byte, ubyte, bool

**16 bit**: short, ushort

**32 bit**: int, uint, float

**64 bit**: long, ulong, double

# Built-in non-scalar types:

Vector

, String, Field, Struct and Table


## Field


| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| name      | field name, only support UpperCamelCase names | true | string |   |
| nonScalar | schema of the field                | true | string | |


```yaml
  name: CreationTime
  description: > 
    # CreationTime
    format: epoch time in nanoseconds
  type:
    io.omniql.base.v1alpha/types/vector:
      type: 
        io.omniql.base.v1alpha/types/ulong:
  reference: io.omniql.base.v1alpha/resource/application
   

```


## Struct 

## Table

apiVersion: io.omniql.base.v1alpha
kind: Types/Table
metadata:
spec:
 fields:

 



Vector of any other type (denoted with [type]). Nesting vectors is not supported, instead you can wrap the inner vector in a table.
string, which may only hold UTF-8 or 7-bit ASCII. For other text encodings or general binary data use vectors ([byte] or [ubyte]) instead.
References to other tables or structs, enums or unions (see below).


> what about union and enums, unions and enum are some of the most important types on omniql, but they will
 not be implement in base, instead they should be defined in `core` due that there are more tools there
 that makes their implementation more easy


