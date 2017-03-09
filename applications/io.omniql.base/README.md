# io.omniql.base.v1alpha

this app contains the omniql predeclared identifiers, the basic building blocks of any omniql schema.

## spec

- base should not depends of any other component(databases, pub/sub system, etc) nor any other application
- is expected that definitions never change at all
- due to the nebtex plan to use flatbuffers extensively, they are first citizen on omniql, this means
  that if we found incompatibilities with other schemas, flatbuffers always will be favored 
- for simplify the initial design spec, defaults will not be supported temporally, 
  this is intended to be addressed by type extensions and build pipelines

## versioning 

  - base does not support automatic versioning, so people should be careful changing this spec (in a perfect world the base app schema should never change) 


### Field

> io.omniql.base.v1alpha/field

- field enable us to define complex data structure

| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| name      | field name,  | true | string |   |
| type      | field type                | true | string | |
| resource  | if this field is a reference to an resource | true | string | |
| description | field short description | true | string | |
| longDescription | field long description support markdown, mermaid, math-jax | true | string | |

### Type

> io.omniql.base.v1alpha/type

scalar types same as flatbuffers

**8 bit**:
 
| Type |              Usage                 |
| ----- | --------------------------------- |
| byte  | io.omniql.base.v1alpha/type/byte  |
| ubyte | io.omniql.base.v1alpha/type/ubyte |
| bool | io.omniql.base.v1alpha/type/bool  |


**16 bit**: short, ushort

| Type |              Usage                 |
| ----- | --------------------------------- |
| short  | io.omniql.base.v1alpha/type/short  |
| ushort | io.omniql.base.v1alpha/type/ushort |

**32 bit**: int, uint, float

| Type |              Usage                 |
| ----- | --------------------------------- |
| int  | io.omniql.base.v1alpha/type/int  |
| uint | io.omniql.base.v1alpha/type/uint |
| float | io.omniql.base.v1alpha/type/float |


**64 bit**: long, ulong, double

| Type |              Usage                 |
| ----- | --------------------------------- |
| long  | io.omniql.base.v1alpha/type/long  |
| ulong | io.omniql.base.v1alpha/type/ulong |
| double | io.omniql.base.v1alpha/type/double |


## Vector

> io.omniql.base.v1alpha/vector


| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| type      | any other type except vectors      | true     | string |          |

Vector of any other type, table, struct or resource, nested vector are not supported


example a vector of ulong

```yaml
  name: CreationTime
  description: > 
    # CreationTime
    format: epoch time in nanoseconds
  type:
    io.omniql.base.v1alpha/type/vector:
      type: 
        io.omniql.base.v1alpha/type/ulong
```


## Table

> io.omniql.base.v1alpha/table

Tables are the main way of defining objects in FlatBuffers, and consist of a name (here Monster) and a list of fields. Each field has a name, a type, and optionally a default value (if omitted, it defaults to 0 / NULL).

Each field is optional: It does not have to appear in the wire representation, and you can choose to omit fields for each individual object. As a result, you have the flexibility to add fields without fear of bloating your data. This design is also FlatBuffer's mechanism for forward and backwards compatibility. Note that:

You can add new fields in the schema ONLY at the end of a table definition. Older data will still read correctly, and give you the default value when read. Older code will simply ignore the new field. If you want to have flexibility to use any order for fields in your schema, you can manually assign ids (much like Protocol Buffers), see the id attribute below.
You cannot delete fields you don't use anymore from the schema, but you can simply stop writing them into your data for almost the same effect. Additionally you can mark them as deprecated as in the example above, which will prevent the generation of accessors in the generated C++, as a way to enforce the field not being used any more. (careful: this may break code!).
You may change field names and table names, if you're ok with your code breaking until you've renamed them there too.

| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| fields| vector of fields |  | [io.omniql.base.v1alpha/field] |  true |


## Struct 

Similar to a table, only now none of the fields are optional (so no defaults either), and fields may not be added or be deprecated. Structs may only contain scalars or other structs. Use this for simple objects where you are very sure no changes will ever be made (as quite clear in the example Vec3). Struct use less memory than tables and are even faster to access (they are always stored in-line in their parent object, and use no virtual table).

> io.omniql.base.v1alpha/struct


| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| fields| vector of fields |  | [io.omniql.base.v1alpha/field] |  true | 


## Enum

> io.omniql.base.v1alpha/enum 

Define a sequence of named constants, each with a given value, or increasing by one from the previous one. The default first value is 0. As you can see in the enum declaration, you specify the underlying integral type of the enum with : (in this case byte), which then determines the type of any fields declared with this enum type.

Typically, enum values should only ever be added, never removed (there is no deprecation for enums). This requires code to handle forwards compatibility itself, by handling unknown enum values.

| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| type| enum type | true  | string |  [io.omniql.base.v1alpha/type/ubyte](#built-in-scalar-type) |
| constants| list of constant | --  | [io.omniql.base.v1alpha/type/enumConstant]|   |

io.omniql.base.v1alpha/enum/constant

| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| name| constant name | true  | string |   |
| description| |   | string |  |
| longDescription | support markdown|  | string |   |


## Union

> io.omniql.base.v1alpha/union 


Unions share a lot of properties with enums, but instead of new names for constants, you use names of tables. You can then declare a union field, which can hold a reference to any of those type, and additionally a hidden field with the suffix _type is generated that holds the corresponding enum value, allowing you to know which type to cast to at runtime.

Unions are a good way to be able to send multiple message type as a FlatBuffer. Note that because a union field is really two fields, it must always be part of a table, it cannot be the root of a FlatBuffer by itself.


| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| types | list of union types | --  | [io.omniql.base.v1alpha/type/unionType]|   |

io.omniql.base.v1alpha/type/unionType


| Name      | Description                        | Required | Schema | Default  |
| --------  | ---------------------------------- | -------- | ------ | -------- |
| type |  application type | true  | string |   |
| resource| | application resource  | string |  |
| description| | | string |  |
| longDescription | support markdown|  | string |   |
