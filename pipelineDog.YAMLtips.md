# Quick Tips On Using JSON-like YAML In PipelineDog Script

============================================================
A PipelineDog Step Definition (as well as the entire Project Definition) is a valid YAML v1.2 file (a superset of JSON), but following a JSON-like style. The decision to use YAML instead of the more popular JSON is to reduce the number of characters (specifically, the `"` character) to type during the encoding of a step, but still use a JSON-like YAML style is to make learning PipelineDog script easier for people who are already familiar with JSON. The specification of YAML v1.2 can be found at [this link](http://yaml.org/spec/1.2/spec.html).

## What is YAML? 
YAML is a human-readable (and quite human friendly, in contrast to machine friendly) language for encoding data. A YAML file is just a plain text file following the YAML language, and describing a collection of key-value pairs (i.e., a key name and a value separated by `:`, for example: `planet: Earth`, the key name is `planet`, and the value for this key is `Earth`).  

## Quick Tips On Using YAML: 

1.1. There must be at least one space after the semicolon (`:`), before the value. Try to avoid using semicolon in your value field.  
1.2. Different key-value pairs should be separated by a comma (`,`), and typed on different lines, for example:
```
planet: Earth,
population: 7125001001
```
Unlike JSON, the last key-value pair can be ended with a comma as well (`population: 7125001001,`).  
1.3. Unlike JSON, enclosing both the key name and the value (both are strings) inside a pair of double quotes (`""`) is not necessary (less typing). However, you can use double quotes (`"..."`) or single quotes (`'...'`) as needed, as YAML will also accept these.  One particular situation where it is desirable to use quotes is when a value contains semicolon: to avoid confusion, quote the entire value string.  
1.4. For more information about YAML, see [here](http://www.yaml.org).  

============================================================
Anbo Zhou  
Yeting Zhang  
Yazhou Sun  
Jinchuan Xing    
May 2016  
Aug 2016

