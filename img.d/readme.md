# How To Include Images Inside a GitHub Markdown File

======

The most straight forward way to add an image:
```HTML
![Alt Image Msg Here](https://github.com/ysunlab/PipelineDog/blob/master/img.d/startPage.jpg?raw=true)
```
And the above code looks like this:  

![Start Page](https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/00startPage.jpg?raw=true)

======

Centered Image:  
<p align="center">
  <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/00startPage.jpg?raw=true" alt="Start Page" />
</p>

The portion above is generated with the following code:
```HTML
Image:  
<p align="center">
  <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/00startPage.jpg?raw=true" alt="Start Page" />
</p>
```

======

For image with a border-like look:
```html
<p align="center">
  <kbd>
    <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/00startPage.jpg?raw=true" alt="Start Page" />
  </kbd>
</p>
```
And it looks like this:  
<p align="center">
  <kbd>
    <img src="https://github.com/ysunlab/PipelineDog/blob/master/img.d/simpleStart.d/00startPage.jpg?raw=true" alt="Start Page" />
  </kbd>
</p>

======
End  
Yazhou Sun Nov 2016
