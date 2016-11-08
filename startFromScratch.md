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
Because we are starting from scratch, you don't need to select or upload any project files (as suggested in the box with dotted line).  

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
  
##4. Modify your List File  
You should modify the content as well as the file name of the List File(s) to suit your needs, and in this example, we will do both.  

  1. You can change the Default List's (or any List File's) name by first hovering over the Default List's tab on the left, and when the Edit icon (a pencil) appears, click on it, and this window pops up, allowing you to modify the List File's name.
  <p align="center">
    <kbd>
      <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/02changeListFileName.jpg?raw=true" alt="Just Started Page" />
    </kbd>
  </p>
  Let's change the Default List's name into `1.txt`.  
  When done, don't forget to clikc the <kbd>OK</kbd> button.  
  2. 
