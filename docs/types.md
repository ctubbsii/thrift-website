---
title: "Thrift Type system"
isdoc: true
---

## Thrift Types
The Thrift type system is intended to allow programmers to use native types as much as possible, no matter what programming language they are working in. This information is based on, and supersedes, the information in the [Thrift Whitepaper](/static/files/thrift-20070401.pdf). The [Thrift IDL](/docs/idl) provides descriptions of the types which are used to generate code for each target language.

### Base Types
The base types were selected with the goal of simplicity and clarity rather than abundance, focusing on the key types available in all programming languages.

 * bool: A boolean value (true or false)
 * byte: An 8-bit signed integer
 * i16: A 16-bit signed integer
 * i32: A 32-bit signed integer
 * i64: A 64-bit signed integer
 * double: A 64-bit floating point number
 * string: A text string encoded using UTF-8 encoding

Note the absence of unsigned integer types. This is due to the fact that there are no native unsigned integer types in many programming languages.

### Special Types
binary: a sequence of unencoded bytes

N.B.: This is currently a specialized form of the string type above, added to provide better interoperability with Java. The current plan-of-record is to elevate this to a base type at some point.

### Structs
Thrift structs define a common object -- they are essentially equivalent to classes in OOP languages, but without inheritance. A struct has a set of strongly typed fields, each with a unique name identifier. Fields may have various annotations (numeric field IDs, optional default values, etc.) that are described in the  [Thrift IDL](/docs/idl).

### Containers
Thrift containers are strongly typed containers that map to commonly used and commonly available container types in most programming languages.

There are three container types:

 * list<type>: An ordered list of elements. Translates to an STL vector, Java ArrayList, native arrays in scripting languages, etc.
 * set<type>: An unordered set of unique elements. Translates to an STL set, Java HashSet, set in Python, etc. Note: PHP does not support sets, so it is treated similar to a List
 * map<type1,type2>: A map of strictly unique keys to values. Translates to an STL map, Java HashMap, PHP associative array, Python/Ruby dictionary, etc.
While defaults are provided, the type mappings are not explicitly fixed. Custom code generator directives have been added to allow substitution of custom types in various destination languages.

Container elements may be of any valid Thrift Type.

N.B.: For maximal compatibility, the key type for map should be a basic type rather than a struct or container type. There are some languages which do not support more complex key types in their native map types. In addition the JSON protocol only supports key types that are base types.

### Exceptions
Exceptions are functionally equivalent to structs, except that they inherit from the native exception base class as appropriate in each target programming language, in order to seamlessly integrate with the native exception handling in any given language.

### Services
Services are defined using Thrift types. Definition of a service is semantically equivalent to defining an interface (or a pure virtual abstract class) in object oriented programming. The Thrift compiler generates fully functional client and server stubs that implement the interface.

A service consists of a set of named functions, each with a list of parameters and a return type.

Note that void is a valid type for a function return, in addition to all other defined Thrift types. Additionally, an oneway modifier keyword may be added to a void function, which will generate code that does not wait for a response. Note that a pure void function will return a response to the client which guarantees that the operation has completed on the server side. With oneway method calls the client will only be guaranteed that the request succeeded at the transport layer. Oneway method calls of the same client may be executed in parallel/out of order by the server.
