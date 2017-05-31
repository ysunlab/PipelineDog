# LEASH Expression Specifications
#### The specifications for LEASH Expressions used in PipelineDog scripts

======
### Table of Content
[1. Basic Definitions](#1)  
[2. LEASH Basics](#2)  
[3. A Quick Example of LEASH](#3)  
[4. A Detailed Guide On How To Use LEASH](#4)  
  [4.1. The *file* keyword](#4-1)  
  [4.2. The *line* keyword](#4-2)  
  [4.3. The *mod* keyword](#4-3)  
  [4.3. The *mods* keyword](#4-4)  
[5. LEASH Reserved Words For Better Usability](#5)  
  [5.1. The `mods` keyword with Reserved Words](#5-1)  
  [5.2. The `$` Reserved Word to Access Other Variables](#5-2)  
[6. Summary](#6)  


## 1. Basic Definitions:
First, some basic definitions:

**List File**: a plain text file containing a list of file paths, with one path on each line.  
**Line Entry**: a line of the List File. This line contains a valid file path.  
**LEASH Target**: a text anchor enclosed inside curly braces, which will be replaced by PipelineDog according to the corresponding LEASH Expression.  
**LEASH Expression**: the detailed instructions on how to replace a LEASH Target with reformatted/reshuffled list entries.  

*Note*: the file path should be following the Unix convention, i.e., something like `/home/SmithJohn/Desktop/1.txt`.

**What is LEASH?**: LEASH stands for Line Entry Automated SHuffling, indicating that the Line Entries will be reformatted and rearranged automatically according to a set of LEASH Expressions.  


## 2. LEASH Basics:
### 2.1. LEASH Keywords

LEASH Expression operates with 3 keywords, and they are:

1. **file**: select one or more List File(s).  
2. **line**: select one or more Line Entries from the selected List File(s), and also specify how these entries should be arranged or repeated.  
3. **mod**: modify each Line Entry with the optional prefix and/or suffix string(s), and also select which basic parts of an entry to retain and modify.  

Additionally, there is a 4th keyword `mods`, which must be used with LEASH Reserved Words ([here](#5)). This `mods` can replace most of the functionalities of `mod` (but not all), and make using LEASH a little easier than using the more powerful `mod` keyword.

### 2.2. The Basic Format of LEASH Expression
 
1. The basic format of a LEASH Expression is `keyword : 'instructions'` or `keyword : "instructions"` (you must use either single quotes or double quotes to enclose your LEASH Expression instructions). Only when the an instruction contains single quotes (`''`), then you must enclose that instruction inside a pair of double quotes (`""`). *Tips*: double quotes always work.  

2. If you have multiple lines of LEASH Expressions, separate each Expression with a comma (`,`):

    ```
    file : '-',
    line : '-'
    ```

3. A set of LEASH Expression collectively instructs PipelineDog to substitute a LEASH target, so the name of this set of LEASH Expression must match the LEASH Target, like the `~B` below:

    ```
    run : /usr/bin/gunzip -c ~A > ~B,
    ~B : {mod : "S'.txt'"},
    ```

4. The name of a set of LEASH Expression must start with a leading `~` (the tilde character), followed by an alphanumerical name. There should be only numbers and letters after the `~` character, any other characters are not allowed. The body of a LEASH Expression must be enclosed inside a pair of curly braces (`{}`).  

5. A LEASH Expression must have a body (i.e., a pair of curly braces), even though there could be nothing in-between the pair of curly braces, e.g., `{}`.  


## 3. A Quick Example of LEASH

First, a simple example with only 1 list file:

If you have a List File (named `p1.txt`) containing these lines:
```
/a/t1.txt
/a/t2.txt
/temp/t3.txt
```

And you have only specified `p1.txt` in your PipelineDog step (i.e., only one input List File for this step).

Then, if you want to generate a command like this:
```
cat /a/t1.txt /a/t2.txt /temp/t3.txt > /temp/all.txt
```

You can use the line below inside a PipelineDog script:
```
cat ~A > ~B
```

Along with the definitions on the LEASH Target `~A` and `~B` below (with some keywords omitted, which will assume the default values for those keywords, see details in later sections):

For target `~A`: (just a single line)  
```
~A : { line : "-:0:' '"}
```  

For target `~B`: (a little bit longer)  
```
~B:{
       line : "3",
       mod : "B'1'S'all.txt'"
}
```
Or in a more human-readable form with `mods`:  
```
~B:{
       line : "3",
       mods : "$..PATH/all.txt"
}
```

After PipelineDog has processed, the string `cat ~A > ~B` becomes:
```
cat /a/t1.txt /a/t2.txt /temp/t3.txt > /temp/all.txt
```


## 4. A Detailed Guide On How To Use LEASH

Here are the detailed explanation of each LEASH Expression keyword, and how to use them:


### 4.1. The *file* keyword:
The `file` is used to select the file(s) for a step.  

This keyword is used in this format: `file : "1-3"`. The value of `file` is a range, e.g., `"1-3"` in this case, or sometime, simply `"-"`.  

The range specification format follows the UNIX range selection convention. For example, if in your PipelineDog script, it has this line:
```
input : [/b/t1.txt, /c/t2.txt, /c/t3.txt, /d/t4.txt]
```
`file : "-"` will select all 4 files (t1 to t4), and this is the default behavior if no `file` component is specified in a LEASH Expression.  
`file : "2-"` will select files from the 2nd to the last (i.e., t2 to t4).  
`file : "-2"` will select files from the first to the 2nd (i.e., t1 and t2).  
`file : "1-3"` will select files 1 to 3 (i.e., t1, t2, and t3).  

*The Default Value of `file`*: when omitted, `file` will take the value of "-", i.e., to select all files.


### 4.2. The *line* keyword:

The `line` is used to select the line(s) in a defined `file` keyword for a step.  

This keyword is used in this format: `line : "1-3"` (again following the UNIX range selection convention). One additional number can be added like this `line : "1-6:3"`, and this number (the Grouping Tag) provided after the colon symbol (i.e., `:`) is to instruct how many entires PipelineDog should place in each constructed command.  

For example, if you have these lines in a selected file:
```
t1
t2
t3
t4
```
And the command containing the LEASH target is this:  
`run : dosth ~A`  

Inside the `~A` LEASH Expression object:  

The default Expression `line : "-"` will select all lines (i.e., Line Entries), then generate output with the `run` command for each line in the file like these:  
```
dosth t1
dosth t2
dosth t3
dosth t4
```

Similar to `file`, lines (i.e., Line Entries) can be selected, such as `line : "1-3"` will produce this:  
```
dosth t1
dosth t2
dosth t3
```

Again, `line : "-"` instructs PipelineDog to select all lines (i.e., all Line Entries). This is also the *default* value for the "line" keyword.  

Now, let's look at the additional (and optional, but when provided, you can do much more with PipelineDog) number after the `:`, i.e., the Grouping Tag. This Grouping Tag (e.g., taking a value of *n*) instructs Pipeline to place *n* number of Line Entries into a single LEASH target instance, i.e., how many lines to put into a single command. The default Expression `line : "-"` is identical to the Expression `line : "-:1"`, i.e., put one Line Entry into one LEASH Target, and repeat the command for all selected lines (i.e., Line Entries). Since there are only four lines in this example List File, the above Expression is also identical to this long Expression: `line : "1-4:1"`. This Expression indicates a Grouping Tag of 1. This will produce this output (actually the same as `"-"`):  
```
dosth t1
dosth t2
dosth t3
dosth t4
```

The *default* value of the Grouping Tag is `1`, so `line : "-"` is equivalent to `line : "-:1"`.  

`line : "-:2"` will select all Line Entries, and then put 2 Line Entries into each LEASH Target, i.e., with a Grouping Tag of 2. This effectively generates a run command for every two lines. PipelineDog will produce this:  
```
dosth t1 t2
dosth t3 t4
```

Notice, there is a space automatically inserted between the 2 Line Entries. What if your program needs the file paths (i.e., Line Entries) separated by some other characters other than the space? Then you can use the Separator Tag like this: `line : "-:2:','"`. A second colon character (`:`) is added to the end of this LEASH instruction, followed by the character (i.e., the Separator Tag) that you want to use instead. The Separator Tag must be enclosed inside a pair of single quotes (if the LEASH instruction is enclosed inside a pair of double quotes). Actually, `line : "-:2:','"` is equivalent to `line : '-:2:","'`, so you just need to let PipelineDog know which part is the entire LEASH instruction, and which part is the Separator Tag.

So `line : "-:2:','"` will produce this:
```
dosth t1,t2
dosth t3,t4
```
And `line : "-:2:';'"` will produce this:
```
dosth t1;t2
dosth t3;t4
```
Be careful, `line : "-:2:''"` will produce this (when the Separator Tag is specified but nothing is provided):
```
dosth t1t2
dosth t3t4
```

The *default* value for the Separator Tag is a space (` `), so `line : "-:2:' '"` is equivalent to `line : "-:2"`.

If the Grouping Tag number cannot be divided evenly by the total number of selected Line Entries, then PipelineDog will use all Line Entries exactly once, and then stop. For example, `line : "-:3"` will produce this result:  
```
dosth t1 t2 t3
dosth t4
```

The Grouping Tag 0, e.g.,`line : "-:0"`, is a special Expression. It will select all Line Entries, and put all Line Entries inside a single command like this:    
```
dosth t1 t2 t3 t4
```
The `0` after `:` in this case instructs PipelineDog to place all selected Line Entries into the same command. Grouping tag 0 means to select all. Also, since there are only 4 lines in the file, in this case the Expression `line : "-:0"` is identical to the Expression `line : "-:4"`. Or in the longest possible format: `line : "1-4:4:' '"`.   

*The Default Value of `line`*: when omitted, "line" will take the value of "-", i.e., to select all Line Entries in the selected files, and to output one Line Entry per constructed command (i.e., with Grouping Tag of `1`, like this `"-:1"`).  


### 4.3. The *mod* keyword:

`mod` stands for modification, and it allows the addition of a prefix (the `P` tag), a suffix (the `S` tag), and the selection of basic parts (the `L` or `F` tag) of the path. The format for specifying how to modify with a specific tag is a tag letter followed by a string enclosed in a pair of single quotes, for example: `P'-i '`. 

First, some quick examples on how to use these tags to modify a Line Entry. For a Line Entry (i.e., line) of `/temp/t3.txt`:  
  1. *Using the `P` tag to add prefix*: `P'Prefix'` will add `Prefix` to a line entry. For example: `mod : "P'-i '"` will produce `-i /temp/t3.txt`. **Pay attention**: if you want to add a space, then you need to include a space in the string enclosed in a pair of single quotes.  
  2. *Using the `S` tag to add suffix*: `S'Suffix'` will add `Suffix` to a line entry. For example: `mod : "S'.exe'"` will produce `/temp/t3.txt.exe`.  
  3. *Using the `L` tag to select the levels (or layers) of a path (i.e. a Line Entry)*, and then to add prefix and/or suffix: `mod :"P'-o 'L'1'S'dosth.exe'"` will produce `-o /temp/dosth.exe`.  

The `P` and `S` tags are easy to use. Anything specified in a pair of single quotes will be added the line entry either as a prefix, or suffix. The `L` tag (L stands for the levels of a path) is slightly more complicated:

More specifics on the `L`, i.e., the basic part selection of a path:  
For a path `/a/b/c/d/e.exe`  
`L'1'` will select `/a`.  
`L'1-3'` will select `/a/b/c`.  
So the basic parts (i.e., /a, /b, /c ...) are numbered from left to right, beginning with 1.  

*Note*: `mod : "L'1'S'dosth.exe'` and `mod : "L'1'S'/dosth.exe'` will produce the same result: `/temp/dosth.exe`, the reason being if PipelineDog did not detect a `S` tag value starting with a forward slash character (`/`), but is used together with a `L` tag, PipelineDog will automatically add a forward slash character (`/`).

Additionally, a `F` tag could be used to specify the file name's parts, instead of the absolute file path's levels (i.e., the `L` tag):  
For a path `/a/b/c/d/e.abc.exe`  
`F'-'` will produce `e.abc.exe`  
`F'1-2'` will produce `e.abc`  

Mixing `L` and `F` together, then you can have:  
For a path `/a/b/c/d/e.abc.exe`  
`L'1-3'F'1-2'` will produce `/a/b/c/e.abc`.  
Again, PipelineDog will automatically add the last file separator (i.e., the forward slash character `/`) at the beginning of the `F` tag value, when a `F` tag is used after a `L` tag, for example:  
`L'1'F'1-2'` will produce `/a/e.abc`.  

*The Default Value of `mod`*: when omitted, `mod` will take the value of `P''B'-'F'-'S''`, i.e., keep the original Line Entry intact when it is placed into the constructed command.


### 4.4. The *mods* keyword:

`mods` (stands for mod-simplified) fulfills most of the functions of `mod` (but not all), but with a much easier-to-remember syntax. `mods` is used together with the LEASH Reserved Words, so the detailed usage of `mods` in the Section 5 [below](#5).


## 5. LEASH Reserved Words For Better Usability:

First, let's assume again a List File (named `p1.txt`) containing these lines:
```
/a/t1.txt
/a/t2.txt
/temp/t3.txt
```

And you have only specified `p1.txt` in your PipelineDog step (i.e., only one input List File for this step).  

With this assumption, let's look at how to use some LEASH Reserved Words together with `mods`.  

These Reserved Words, when used together with the keyword `mods`, can make programming in PipelineDog with LEASH easier, and some of these words are:  

1. `$LINE` : just indicating a Line Entry in the List File (e.g., `/temp/t3.txt`)  
2. `$PATH` : just the path to the folder containing the file in a Line Entry (e.g., `/temp`)  
3. `$FILENAME` : just the filename of a Line Entry, excluding the path (e.g., `t3.txt`)  
4. `$..PATH` : the path to the parental folder of `$PATH` (e.g., `/`)  
5. `$FILENAME_WITHOUT_EXTENSION` : just the filename, excluding the last suffix (e.g., `t3`)  


### 5.1. The `mods` keyword with Reserved Words  

In a nutshell, `mods` is a simplified version of `mod`, and let's see an example first:  
If we want to generate this output based on the List File `p1.txt` using LEASH:  
```
cat /a/t1.txt /a/t2.txt /temp/t3.txt > /temp/all.txt
```
We can use a script like this with PipelineDog by using `mod`:
```
run : cat ~A > ~B
~A : {line : "-:0"}
~B : {
line : "3",
mod : "L'1'S'all.txt'"
}
```
Or, we can use this script by using `mods`:
```
run : cat ~A > ~B
~A : {line : "-:0"}
~B : {
line : "3",
mods : "$PATH/all.txt"
}
```
So `mods` is using a format similar to what you would want to see in the actual constructed command. Here are some additional examples (assuming working on the Line Entry `/temp/t3.txt`):  
To get `-i /temp/t3.txt`, the `mods` instruction is `mods : "-i $LINE"`;  
To get `-o /temp/t3.txt.out`, the `mods` instruction is `mods : "-o $LINE.out"`;  
To get `-o /temp/t3.out`, the `mods` instruction is `mods : "-o $PATH/$FILENAME_WITHOUT_EXTENSION.out"`;  

In essence, the `P`, `S`, `L`, and `F` tags of `mod` have been replaced by string literals typed inside a `mods` instruction. It is obvious with only a limited set of Reserved Words, `mods` cannot replace all functions of `mod`. But for the most frequently encountered cases, `mods` can be used instead of `mod` with a set of much easier-to-remember Reserved Words.  

**Important**: `mods` cannot be used inside the same LEASH object with `mod`. If there are both `mod` and `mods` inside the same LEASH object, for example:
```
mod : "L'1'S'all.txt'",
mods : "$..PATH/all.txt"
```
Then PipelineDog will only follow the instructions provided inside `mod`, and ignore `mods`.  


### 5.2. The `$` Reserved Word to Access Other Variables  

The `$` is also a Reserved Word of LEASH, and it can be used to access other key-value pairs (YAML variables). We have already seen an example in the PipelineDog Step Definition document:  
```
2-1 : {
name : Gunzip while keep original,
in : 1.txt,
run : /usr/bin/gunzip -c ~A > ~B,
~A : {},
~B : {mod : "S'.txt'"},
out : $~B,
}
```
Where the `out` key takes on the value of the `~B` key. Similar to its UNIX/Linux concept, the `$` Reserved Word in this case also has the meaning of "taking the value of". This `$` allows code reuse, and as a result, increase productivity.  

With `$`, you can also reorganize your PipelineScript to separate your code into one part that need to be modified for each pipeline or each run, and a second part that will remain the same most of the time. Here's an example:
```
gunzipC : {
myIn : [1.txt, 2.txt, 3.txt],
myStepNum : 2-1,
myPath : /usr/local/bin
}

$gunzipC.myStepNum : {
name : Gunzip while keep original,
in : $g1.myIn,
run : $g1.myPath/gunzip -c ~A > ~B,
~A : {},
~B : {mod : "S'.txt'"},
out : $~B,
}
```
In the above example, we have combined all the values that probably will change from run to run into a single object called `gunzipC`, and its fields `myIn`, `myStepNum`, and `myPath` contains the information of the actual input List Files, the Step-ID in the current pipeline, and the path to the program to run. Later, in the actual Step Definition for "Gunzip while keep original", these values can be accessed by `$gunzipC.myStepNum`, `$gunzipC.myIn`, and `$gunzipC.myPath`. The PipelineDog program has a pre-processor will replace all these tags with a leading `$` with the actual variable values that they point to. So after the PipelineDog pre-processor (automated and hidden from the user), your code actually becomes this:
```
2-1 : {
name : Gunzip while keep original,
in : 1.txt,
run : /usr/bin/gunzip -c ~A > ~B,
~A : {},
~B : {mod : "S'.txt'"},
out : {mod : "S'.txt'"},
}
```
Notice, all the tags with a leading `$` have been replaced by the actual values.


## 6. Summary

The LEASH Expression provides an automated way to reformat and rearrange Line Entries, and it relies on only three keywords: `file`, `line`, and `mod` (or sometimes, `mods`). `file` provides the selection of List Files, or a Step Output object. `line` provides the selection of Line Entries, and it also provides options on how many to select for each constructed command, and how to arrangement them with separator characters. `mod` provides the ability to modify the Line Entries in a specific way, as described in the `mod` instruction. `mods` provides almost the same functionality of `mod`, but in a slightly easier-to-remember format.

You can find out more examples of LEASH in the PipelineDog Step Definition  [here](https://github.com/yazhousun/PipelineDog/blob/master/pipelineDog.StepFormatDefinition.md) and the PipelineDog Project Definition  [here](https://github.com/yazhousun/PipelineDog/blob/master/pipelineDog.ProjectFormatDefinition.md).

============================================================
Anbo Zhou  
Yeting Zhang  
Yazhou Sun  
Jinchuan Xing  
May 2016     
August 2016

