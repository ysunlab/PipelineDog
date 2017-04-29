# PipelineDog (Web) Step Definition File Specification

======  

### Table of Content
[1. A Quick Overview](#1)  
[2. The Detailed Specification](#2)  
  [2.1. The Step-ID Property](#2-1)  
  [2.2. The Name Property](#2-2)  
  [2.3. The In Property](#2-3)  
  [2.4. The Run Property](#2-4)  
  [2.5. The LEASH Expression Property](#2-5)  
  [2.6. The Out Property](#2-6)  
  [2.7. The Comment Property](#2-7)  
[3. Summary](#3)  

This document specifies the format of a PipelineDog (Web) Step Definition file (to be used in the PipelineDog Web App at web.pipeline.dog). Multiple PipelineDog Step Definition files can then be used to created a PipelineDog Project Definition file (an example can be seen [here](https://github.com/ysunlab/PipelineDog/blob/master/web.pipelineDog.ProjectFormatDefinition.md)). The PipelineDog Project Definition file can be used to create a executable bash script to run the analysis pipeline on your local system. This PipelineDog Project Definition file is thus a record of your pipeline run containing all the details about the input files, output files, commands, and the order these commands were carried out, and it is much easier to read and maintain than the generated bash script. The individual steps of a pipeline, i.e., a PipelineDog Step Definition file, can then be re-used efficiently to create other pipelines.   

First, some basic definitions:  

**List File**: a plain text file containing a list of file paths, with the path of each file on a separate line. You typically upload the List Files to the PipelineDog Web App (web.pipeline.dog), or directly create such files in the Web App.  
**Line Entry**: a line of the List File. This line contains the path of a file on your local system (yes, the end product of PipelineDog is a bash script that you can run on your local system, not inside the PipelineDog Web App, so the file path is local to the system where the pipeline bash script is going to run).   
**[LEASH Target](#LEASH-target)**: a text anchor marked by a leading `~` (the tilde character), which will be replaced by PipelineDog according to the corresponding LEASH expression.  
**[LEASH Expression](#LEASH-expression)**: the detailed instructions on how to replace a LEASH target with reformatted/reshuffled Line Entries.  

<a name="1" />
## 1. A Quick Overview

A PipelineDog Step Definition file (as well as the entire Project Definition file) is just an YAML file with PipelineDog-specific tags. YAML is a human-readable language that describes a collection of key-value pairs (i.e., a key name and a value separated by `:`, for example: `planet: Earth`, the key name is `planet`, and the value for this key is `Earth`).  

An example Step Definition:

```
2-1 : {
name : Gunzip while keep original,
in : $1.txt,
run : /usr/bin/gunzip -c ~A > ~B,
~A : {},
~B : {mod : "S'.txt'"},
out : $~B,
}
```

For quick tips on how to use YAML in general and in PipelineDog, please read [here](https://github.com/ysunlab/PipelineDog/blob/master/pipelineDog.YAMLtips.md).  

If the `1.txt` file (i.e., a List File) contains the follow lines (i.e., Line Entries):
```
/a/ABC.gz
/b/CDE.gz
/c/FGH.gz
```
After PipelineDog parsed this Step Definition, the generated bash script will contain the following commands:
```
/usr/bin/gunzip -c /a/ABC.gz > /a/ABC.gz.txt
/usr/bin/gunzip -c /a/CDE.gz > /a/CDE.gz.txt
/usr/bin/gunzip -c /a/FGH.gz > /a/FGH.gz.txt
```
Note: the actual Bash script PipelineDog generated will contain more (usually much more) codes than this, especially when used as part of a pipeline.

============================================================  
<a name="2" />
## 2. The Detailed Specification

Let's examine all the key-value pairs inside this Step Definition one by one.

<a name="2-1" />
### 2.1. The *Step-ID* Key  

  __In our example__: `2-1: {...}`  
  __Required__: Yes  
  __Must be non-empty__: Yes  
  __Function__: Uniquely identify a step object in a pipeline.  
  
  __How to use__:  
  1. The naming of a *Step-ID* key must following this format: `DD-dd`.
  2. This key name must start with a number, have a dash (`-`) after the first number, and end with another number. The name should not contain any other characters other than numbers and dash. 
  4. `DD`, a positive integer, represents the order or sequence of a step should take place inside a pipeline. If this is the first step, then `1-dd`, and if this is the second step, then `2-dd`.
  5. `dd`, also a positive integer, represents the different tasks that must be performed in each step. For example, if there are two tasks that must be performed in the first step, then you would have two step objects, one named `1-1` and the other named `1-2`.
  6. The key value (i.e., things between the `{}`) must be non-empty. you wouldn't define a step inside a pipeline that does nothing, right?

<a name="2-2" />
### 2.2. The *Name* Key   

  __In our example__: `name : Gunzip while keep original,`  
  __Required__: Yes  
  __Must be non-empty__: Yes  
  __Function__: Describe (to yourself and future users) the nature/function of this analysis step.  
  
  __How to use__:  
  1. The key name must be: `name`.
  2. The key value must be a non-empty string, e.g., `Gunzip while keep original`.
  3. This is mainly for the users to understand what does each step in a pipeline do. So to avoid future confusion, please describe the nature of this step with necessary details.  
  4. *Tips*: notice the comma at the end of the line (`,`). This is required by YAML to separate different key-value pairs inside an object.

<a name="2-3" />
### 2.3. The *In* Key  

  __In our example__: `in : $1.txt,`  
  __Required__: No  
  __Must be non-empty__: Yes  
  __Function__: Provide the name of a List File, or the name of a step-output object (more details about this step-output object are covered later).  
  
  __What is a *List File*__: it is just a plain text file containing one or more lines (i.e., Line Entry), and each line represent a file path (in your local file system) that will be used for this task operation. For example:
```
/e/f/g.d/ABC.fasta
/e/f/g.d/DEF.fasta
/e/f/g.d/GHI.fasta
```
  
  __How to use__:  
  1. The key name must be: `in`.
  2. The key value must be a `$` character followed by a non-empty string, and this string can be:  
    1. a List File name, i.e., an input file (e.g., `1.txt`), which should have been uploaded to the PipelineDog Web App (when uploaded, this file will show up in the List Files tab), or have been created within the PipelineDog Web App (you can click the `+` button in the List Files tab to create a new List File).  
    2. a name identifying a step-output object, e.g., `1-1.out`, or `1-1.out1` if you have defined more than one output object for a step. More details about this Step Output object are covered later.     
  3. If you need more than one List File, then put them one by one into the square brackets (`[]`), and separate them by comma (`,`), (i.e., making them into an array in YAML), and prefix the `$` before each of the names, for example: `[$1.txt, $2.txt, $3.txt]`.  
  4. Yes, you can mix List Files and Step Output objects:  
`[$1-1.out, $2.txt, $3.txt]`.  
  5. When the `in` key is omitted, then an analysis step without any input files has been defined. Examples of this kind of input-less analysis steps are: to check the disk free space with `df`, or to check remote host availability with `ping`. An input-less step might have an output object (e.g., redirect the `df` output to a file).  
  6. The `$` is to instruct PipelineDog to take the content (or value) of the List File, similar to the UNIX command line usage of `$`.  

<a name="2-4" />
### 2.4. The *Run* Key  

  __In our example__: `run : /usr/bin/gunzip -c ~A > ~B,`  
  __Required__: Yes  
  __Must be non-empty__: Yes  
  __Function__: Provide the template for PipelineDog to automatically construct one or more commands to run this analysis step in your local system.  
  
  __How to use__:  
  1. The key name must be: `run`.  
  2. The key value must be a non-empty string. This string is a plain UNIX/Linux command-line command, but replace the actual file paths with LEASH Targets, e.g., the `~A` and `~B` tags here inside `/usr/bin/gunzip -c ~A > ~B`.  
  3. What's a LEASH Target? <a name="LEASH-target" />  
  *Let's look at this simple example:*  
  To check the content of a folder, you would issue this command in your command line environment: `ls -alsht /usr/bin`: the actual file path (in this case `/usr/bin`) needs to be changed every time you want to check a new folder. How NOT to hard-code the file path into a Step Definition file? Use LEASH expression together with PipelineDog! LEASH stands for *Line Entry Automated SHuffling*. Remember the *In* key-value pair that we have already set up? That *In* key's value is a list of file paths (Line Entries) to the actual data file in your local file system (contained within a List File or several List Files). PipelineDog will automatically substitute these LEASH Targets with Line Entries (i.e., file paths) stored in List Files, in ways that have been specified by the corresponding LEASH expression, as described in the next section.   
  4. A LEASH Target must start with a leading `~` (the tilde character), followed by a alphanumerical name. There should be only numbers and letters (and the `~` character) for a LEASH Target, while any other characters are not allowed.  
  
<a name="2-5" />
### 2.5. The *LEASH Expression* Key  
<a name="LEASH-expression" />  
  __In our example__: `~B : {mod : "S'.txt'"},`  
  __Required__: Yes, if a corresponding LEASH Target has been used  
  __Must be non-empty__: No, it can be an empty pair of curly braces `{}`, like `~A : {},` and in this case, this LEASH target will take on all the default values of a LEASH Expression.  
  __Function__: Provide specific instructions to PipelineDog on how to replace the LEASH Targets inside the *Run* template with real paths.   
  
  __How to use__:  
  1. The key name of a *LEASH Expression* must be matching a LEASH Target specified in the *Run* value: `~A` and `~B` matches the two targets in `/usr/bin/gzip -c ~A > ~B`.  
  2. The key value must be enclosed inside a pair of curly braces: `{}`. This is the body of a LEASH expression, and it instructs PipelineDog on how to replace the LEASH Targets with real paths. The details on how to construct LEASH Expression are provided [here](https://github.com/ysunlab/PipelineDog/blob/master/pipelineDog.LEASHexpression.md).  
  3. Here, `~A : {},` instructs PipelineDog to replace the `~A` element with the file paths stored in `1.txt` using default values (as nothing was provide between `{` and `}`), and the behavior is one Line Entry for each command constructed. Similarly, `~B : {mod : "S'.txt'"},` instructs PipelineDog to replace the `~B` element with the file paths stored in `1.txt`, one Line Entry for each command constructed, but this time each Line Entry has been appended with the suffix `.txt` (the `S` tag). After PipelineDog has parsed the code completely, inside the resulting Bash script ready to use, these are the actual commands that will be run:
```
/usr/bin/gzip -c /a/ABC.gz > /a/ABC.gz.txt
/usr/bin/gzip -c /a/CDE.gz > /a/CDE.gz.txt
/usr/bin/gzip -c /a/FGH.gz > /a/FGH.gz.txt
```


<a name="2-6" />
### 2.6. The *Out* Key  

  __In our example__:`out : $~B,`  
  __Required__: No  
  __Must be non-empty__: No, it can be an empty pair of curly braces `{}`  
  __Function__: Specify the list of new files that will be generated after this step has been successfully executed (so that later steps can get access to this list of new Line Entries)
  
  __How to use__:  
  1. The key name must be: `out`, if there is only one set of output files generated. If there are multiple sets of output files generated, then name each set as `out1: {...}`, `out2: {...}`, and so on. Or more specifically, the key name of *Out* must be start with `out`, and can be followed by one ore more digits if there are more than one set of output files. I.e., in this format: `out`, `outD`, `outDD`, or 'outDDD', ..., where `D` is a digit.  
  2. The key value must be enclosed inside a pair of curly braces: `{}`. This is the body of a LEASH Expression, and this LEASH Expression does not replace some existing LEASH Target, but instead create a set of output files (more precisely, output file paths, i.e., Line Entries) that can be used by later analysis steps.  
  3. If the LEASH Expression body is completely identical to a previous LEASH Expression, then you can just refer to the other LEASH Expression by its name (with a leading `$`), for example, in this case: `out : $~B,`. This `out : $~B,` is equivalent to `out : {mod : "S'.txt'"},`. The `$` is to instruct PipelineDog to take the content (or value) of that LEASH Expression, similar to the UNIX command line usage of `$`.  
  4. If one step (and its Step ID value is `1-1`) has an *Out* key named `out`, then later steps (e.g., `2-1` and later ones) can use the Line Entries in this `out` property by using `$1-1.out` in their `in` property, e.g., `in : $1-1.out`. If this step (`1-1`) has more than one `out` objects (e.g., `out1` and `out2`), then they can be used as `in : [$1-1.out1, $1-1.out2]`.  
  5. When the `out` key is omitted, then an analysis step without any output files has been defined. Examples of this kind of output-less analysis steps are: commands to shutdown/terminate remote clouding computing instances (e.g., `terminate-instances` for EC2), or commands to perform local or remote backup. An output-less step might have an input key (e.g., specifying which files to backup).  
  
<a name="2-7" />
### 2.7. The *Comment* Key  

  __In our example__: we didn't include a comment field in our example, but a comment example could be: `comment : Created by John Smith for XYZ project on 2016/01/31 using ...`   
  __Required__: No  
  __Must be non-empty__: No  
  __Function__: Provide additional information about this step for the users (typically when it is too long to put inside the *name* field)
  
  __How to use__:  
  1. The key name must be: `comment`.  
  2. The key value can be anything (PipelineDog will not process the comment field).  

<a name="3" />
## 3. Summary
There are in total 7 different kinds of key-value pairs in a Step Definition, and they are explained in details in 2.1 to 2.7. There are 6 keys to describing a step (2.2 to 2.7), and one to ID the entire step (2.1). With a step defined, you can then define a pipeline with several steps. You can find out how to define a pipeline in the PipelineDog Project Definition file  [here](https://github.com/ysunlab/PipelineDog/blob/master/web.pipelineDog.ProjectFormatDefinition.md).

A quick reference, the simplest Pipeline step definition contains:
```
1-1 : {
name : a text string to describe this step,
in : a string to describe either a file or a step output object, prefixed with a $,
run : a string to describe a command template with some LEASH ~target,
~Target : {an object: the LEASH Expressions for this target},
out : {an object: using LEASH Expression to define the output object},
comment : additional information this step
}
```
And the example definition file we have used here:

```
2-1 : {
name : Gunzip while keep original,
in : $1.txt,
run : /usr/bin/gunzip -c ~A > ~B,
~A : {},
~B : {mod : "S'.txt'"},
out : $~B,
}
```

In total, there are only 3 required key-value pairs that cannot be missing inside a Step Definition, and they are: the *Step-ID* key (`1-1`), the *Name* key (`name`), and the *Run* key(`run`). A LEASH target (`~A`) and its corresponding LEASH expression are the key mechanisim that allows PipelineDog to automatcally format and insert input file paths and output file paths to the appropriate place in a command line argument list.

To learn more details about the LEASH expression, read [here](https://github.com/ysunlab/PipelineDog/blob/master/pipelineDog.LEASHexpression.md).

============================================================   
Anbo Zhou  
Yeting Zhang  
Yazhou Sun  
Jinchuan Xing    
May 2016  
Aug 2016
Oct 2016
