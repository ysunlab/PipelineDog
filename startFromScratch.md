# PipelineDog Web App Guide: Start From Scratch

======

This tutorial introduces a simple example to use PipelineDog Web App. You should have some knowledge of the Unix/Linux command-line operations, and have access to a machine with bash (a Unix shell).  

##1. With a compatible web browser, go to www.pipeline.dog
Visit this web address (www.pipeline.dog) with a compatible web browser, and you have reached the PipelineDog Web App. The current list of tested compatible web browers are: Safari (v 10.0.1 or above), Opera (v 40.0 or above), and Chrome (v 54.0 or above).  

You should arrive a web page similar to this one:  
<p align="center">
  <kbd>
    <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/00startPage.jpg?raw=true" alt="Start Page" />
  </kbd>
</p>
And this is the starting page for the PipelineDog Web App.  

##2. Optional (but recommended): read the PipelineDog documentations
The documentations specifically for PipelineDog are here: https://github.com/ysunlab/PipelineDog  

##3. Start a new project by clicking the <kbd>START</kbd> button  
Because we are starting from scratch, you don't need to select or upload any project files (as suggested in the box with dotted line). In this project, we will use the example that we have also used in our PipelineDog definition documentations (e.g., [here](https://github.com/ysunlab/PipelineDog/blob/master/web.pipelineDog.StepFormatDefinition.md) and [here](https://github.com/ysunlab/PipelineDog/blob/master/web.pipelineDog.ProjectFormatDefinition.md)).  

In this example, we want to compare the file sizes of gzipped version, bzip2 compressed version, and the uncompressed original version (so that, for example, one would then know the compression ratio, and then decide which compressor to use for data backup/transmission). We start with some gzipped files (e.g., downloaded from a server), we decompress these gzipped files to recover the original files (while keeping the original gzipped versions), compress the uncompressed ones again but with bzip2 (again, keep the original uncompressed versions intact), and finally get the file sizes for all three versions of the file (or files, if you start with multiple gzipped files as in our example), and write the size information to separate files.  

##4. Know a little bit about the PipelineDog Web App interface  
After you have clicked the <kbd>START</kbd> button, you should land on the following page:  
<p align="center">
  <kbd>
    <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/01justStarted.jpg?raw=true" alt="Just Started Page" />
  </kbd>
</p>

The interface is divided into two large parts, a panel part on the left side, and a larger editor part on the right.  

  1. On the left, from top to bottom, there is a <kbd>Global Variables</kbd> section (we will not use in this simple example), the <kbd>List Files</kbd> tab (a <kbd>Default List</kbd> file has already been created for you, and pre-selected by default, i.e., with a gray background), and the <kbd>Steps</kbd> tab (a <kbd>Default Step</kbd> has been created for you).  

  2. On the right, the <kbd>EDITOR</kbd> window now display the content of the pre-selected <kbd>Default List</kbd>. Initially, it contains the following content:  
  ```
  /home/usr/b1.bam
  /home/usr/b2.bam
  /home/usr/b3.bam
  ```
  You should delete these lines, and replace them with real file paths on your local system that will be used as the pipeline input.  
  There is no <kbd>SAVE</kbd> button, as this Web App automatically saves all your changes. If you just want to make sure, click on the PARSE button, i.e., the <kbd>&lt; &gt;</kbd> button, so that you force a save event.  
  
  3. Additionally, you can upload a List File by clicking the <kbd>&#8613;</kbd> button, and you can also download the current List File that you have selected (i.e., with a gray background) by clicking the <kbd>&#8615;</kbd> button.  
  
##5. Modify your List File  
You should modify the content as well as the file name of the List File(s) to suit your needs, and in this example, we will do both.  

  1. You can change the <kbd>Default List</kbd>'s (or any List File's) name by first hovering over the <kbd>Default List</kbd>'s tab on the left, and when the Edit icon (a pencil) appears, click on it, and this window pops up, allowing you to modify the List File's name.
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/02changeListFileName.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  Let's change the <kbd>Default List</kbd>'s name into `1.txt`.  
  When done, don't forget to clikc the <kbd>OK</kbd> button.  
  2. You can change the content of the <kbd>1.txt</kbd> List File by using the <kbd>EDITOR</kbd> window on the right. For example, assume that you have the following gzipped files with the following path:  
  ```
  /home/jsmith/Desktop/t1.gzip
  /home/jsmith/Desktop/t2.gzip
  /home/jsmith/Desktop/t3.gzip
  ```
  
  Again, there is no <kbd>SAVE</kbd> button, as this Web App automatically saves all your changes. If you just want to make sure, click on the PARSE button, i.e., the <kbd>&lt; &gt;</kbd> button, so that you force a save event.  
  Now, your PipelineDog Web App window looks like this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/03modifiedListFile.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>

##6. Add a PipelineDog step  
Now you can add an analysis step (read more about PipelineDog step definition [here](https://github.com/ysunlab/PipelineDog/blob/master/web.pipelineDog.StepFormatDefinition.md)) by clicking on the <kbd>Default Step</kbd> in the <kbd>Steps</kbd> tab on the left.  
  
  1. Initially the <kbd>Default Step</kbd> has no content, so it looks like this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/04stepEditorDefault.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
  2. Let's enter a step definition into the <kbd>EDITOR</kbd> window on the right. Let's use the example also shown in our PipelineDog step definition documentation, i.e., unzip gzipped files while keeping the original gzipped ones. The code that we will be using is the following:  
  ```
  1-1 : {
      name : Gunzip while keep original,
      in : $1.txt,
      run : gunzip -c ~A > ~B,
      ~A : {},
      ~B : {mod : "S'.txt'"},
      out : $~B
  }
  ```
  
  After some typing, your window should look like this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/05justEnteredStep1Def.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
  3. You will notice that although we have specified the step name by using `name : Gunzip while keep original`, at this moment in the  <kbd>Steps</kbd> tab on the left this step is still named `Default Step`. This is because although the saving of your code is automatic, you need to parse the code by clicking the PARSE button, i.e., the <kbd>&lt; &gt;</kbd> button. After you have clicked the <kbd>&lt; &gt;</kbd> button, your window now looks like this (i.e., with the correct step name displayed on the left):  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/06step1Parsed.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
  4. After your step definition code has been parsed, you can now view the commands that will be generated according to your step definition code. To view the commands, click on the <kbd>COMMAND</kbd> button on the very top your right-side panel. And after clicking it, you should see this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/07step1Command.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
  5. Also, you can view the output of this step (as defined by the `out` tag in your code). TO view the output, click on the <kbd>OUTPUT</kbd> button on the very top your right-side panel. And after clicking it, you should see this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/08step1Output.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
##7. Add more steps
Typically an analysis pipeline has more than one step, and you can add additional steps by clicking the ADD button, i.e., the <kbd>+</kbd> button, next to the <kbd>Steps</kbd> tab on the left. After clicking the <kbd>+</kbd> button, a new <kbd>Unamed Step</kbd> will appear on the left, and following the instructions in Step 6 above, you can modify the content, and then add other steps if necessary. What we will add in this example are the following:  

  1. Add a step where we use `bzip2` to compress the output files from step `1-1`. The code to use is the following:  
  ```
  2-1 : {
      name : Bzip2 while keep original,
      in : $1-1.out,
      run : bzip2 -k ~A,
      ~A : {},
      out : {mod : "S'.bz2'"}
  }
  ```
  
  And after you have entered the above code (step `2-1`), parsed the code, your window should look like this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/09addStep2.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
  2. Finally, we add a step to get all the file sizes (including the original gzipped ones, the decompressed ones, and the bzip2 compressed ones), and write the file size information to seperate text files retaining the original file name, but with a new suffix `.ds` (stands for data size). The code to use in the final step is the following:  
  ```
  3-1 : {
      name : Check file size and save to file,
      in : [$1.txt, $1-1.out, $2-1.out],
      run : du ~A > ~B,
      ~A : {},
      ~B : {mod : "S'.ds'"}
  }
  ```
  And after you have entered the above code (step `3-1`), parsed the code, your window should look like this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/10addStep3.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
##8. Save project & output bash script  
At the moment, you have entered all the code for this pipeline, and specified the input data files' file paths. Now we can get the bash code that will run this pipeline on your local system. Just in case, let's first save the project.  
  
  1. To either save a project or export the generated bash script, you should click on the <kbd> &#8942; </kbd> button on the very top-right corner of the PipelineDog interface, and you will get this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/11saveProjOrExportPipeline.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
  2. To save a project, click the <kbd>Save Project</kbd> button, and you will get this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/13saveProject.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
  With the above window shown, you can either download the entire PipelineDog script by clicking the <kbd>DOWNLOAD</kbd> button in this poped-up window, or copy-paste the content inside this window directly.  
  
  3. To export the bash script (so you can run this script locally to actually run this pipeline and get the results), click the the <kbd>Export Pipeline</kbd> button, and you will get this:  
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/12exportPipeline.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  
  Again, with the above window shown, you can either download the entire PipelineDog script by clicking the <kbd>DOWNLOAD</kbd> button in this poped-up window, or copy-paste the content inside this window directly.  
  
  4. After you have obtained the generated bash script on your local system (and let's assume this script is named `p1.bash`), you can simple run this script by the following command:  
  ```
  ./p1.bash
  ```
  
  Usually, you need to change the just created script from a text file to a runnable script, and you should use the following command:  
  ```
  chmod +x p1.bash
  ```
  
  And if you have been following our example, you can check all the file sizes by issuing this command:  
  ```
  cat ./*.ds
  ```

##9. Restart for another pipeline  
If you have done editing, saving, and exporting this pipeline, and you want to start over with another pipeline, then you can click on <kbd> &#9776; </kbd> button on the very top-left corner of the PipelineDog interface, and you will get this:  
<p align="center">
  <kbd>
    <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/14mainMenu.jpg?raw=true" alt="Just Started Page" />
  </kbd>
</p>

Among the several options listed on the poped-up menu, there is the <kbd>New Project</kbd> button. Once clicked, you will be able to start from scratch again.

======

Yazhou Sun  
Anbo Zhou  
Yeting Zhang  
Jinchuan Xing  

Nov 2016  
