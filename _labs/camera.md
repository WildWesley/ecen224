---
title: Camera
number: 8
layout: lab
---

## Objectives

- Take a picture with the camera
- Handle raw data in a buffer
- Write a `.bmp` file
- Update viewer interface

## Getting Started

Use the GitHub Classroom link posted in the Learning Suite for the lab to accept the assignment. Next, ssh into your Raspberry Pi using VSCode and clone the repository in your home directory. **This lab should be done in VSCode on your Raspberry Pi. Make sure the lab is top level folder of your VSCode editor.**

## Overview

In the past labs, much time has been spent making sure the files of the camera are easily accessible and represented on the LCD screen of your Pi Z2W kits. In this lab you will be expected to integrate a camera module to your design and take pictures. 

In the [Getting Started Lab]({% link _labs/getting-started.md %}), you should have already physically installed your camera, using the **longer** of the two ribbon cables in the correct orientation.

### Creating Large Buffers
The skills that you need to complete this lab are very closely related to the skills you needed for the last lab. You are expected to capture a very large buffer of information that comes from a camera and store it in a buffer (`uint8_t *`) in C. 

Normally, if we knew the size of these buffers, we would be able to create a regular array in C:

```c
uint8_t my_new_buf[256];    // Create a buffer of a known size
```

However, this becomes difficult when our buffers become very large. For example, the buffers that you dealt with in the **Image** lab had millions of values in it! This is much too large for variables that are declared on the stack. Instead, you will need to use `malloc()` in conjunction with an unbounded array:

```c
uint8_t * my_new_buf = malloc(sizeof(uint8_t) * SIZEOFYOURBUFFER);

// Whenever we are done with a malloc'd buffer in C, don't forget to call the following line:
free(my_new_buf);
```

Malloc takes one argument, the number of bytes to allocate for the buffer. It is common to provide the size of the data type the buffer will be (in this case, `uint8_t` so `sizeof(uint8_t)`) and the number of elements you want. This creates your buffer in the *heap* where there is much more space for larger variables like this. However, whenever we declare something in C and manage its memory, we must remember to call `free()` on the object once we are done. If this is not done, then there will be memory leaks inside of your program which could potentially grind your system to a halt.

### Writing to a File
You'll notice in your lab files for this lab, you are provided with a `camera.h` and a partially filled in `camera.c`.

In this lab you will be expected to capture image data from a camera, save it to a file, and then show it on the screen of your new smart doorbell. In order to write information to files in C, you will be expected to know one more function than you already do: `fwrite()`.

Much like reading a files as you did in the last lab, to write, you will need to open a file using `fopen()`. You may be thinking, how can I open a file when it doesn't exist yet? With `fopen()` you can name the file you want to create and what kind of mode you want to create it in by using `fopen()`. For example, to create a new file named "banana.txt", I would do the following:

```c
FILE *fp;
fp = fopen("banana.txt", "w");
```

The second argument in `fopen()` is mode we want to open the file in. This allows the operating system to know what type of content will be written to the file. In our example above, we want to write ASCII text to the file (as implied with the `.txt` extension), so we use the `w` mode. Other special modes for `fopen()` can be seen in the following table below.

| Mode | Description                                                                                                                                                                                                                                                                                                                                                                                                     |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `r`  | Open text file for reading.  The stream is positioned at the beginning of the file.                                                                                                                                                                                                                                                                                                                             |
| `r+` | Open for reading and writing.  The stream is positioned at the beginning of the file.                                                                                                                                                                                                                                                                                                                           |
| `w`  | Truncate file to zero length or create text file for writing.  The stream is positioned at the beginning of the file.                                                                                                                                                                                                                                                                                           |
| `w+` | Open for reading and writing.  The file is created if it does not exist, otherwise it is truncated.  The stream is positioned at the beginning of the file.                                                                                                                                                                                                                                                     |
| `a`  | Open for appending (writing at end of file).  The file is created if it does not exist.  The stream is positioned at the end of the file.                                                                                                                                                                                                                                                                       |
| `a+` | Open for reading and appending (writing at end of file). The file is created if it does not exist.  Output is always appended to the end of the file.  POSIX is silent on what the initial read position is when using this mode. For glibc, the initial file position for reading is at the beginning of the file, but for Android/BSD/MacOS, the initial file position for reading is at the end of the file. |

It is considered good practice to add a `b` at the end of the letter if you intend to write to a file in a binary format (i.e. `.bmp` files) to indicate your intent, however this extra `b` is not enforced on many modern systems like Linux.

After the file is opened, you can then use the `fwrite()` command to write to this opened file. Instructions on how to use `fwrite()` can be found in the corresponding link in the **Explore More!** section at the bottom of the page. Don't forget to use `fclose()` on the file pointer when you are done writing the file!

## Requirements

- First verify your camera is working. To do that, run the following command:

```
libcamera-still -n --immediate -e bmp --width 128 --height 128 -o camera-test.bmp
```

If this command succeeds, that means you have correctly connected the camera. If the command fails, such as a camera not found error, then you need to fix the connection.
  
- Copy your code in `main.c` from the previous lab into this lab's `main.c`.

- Assign the right button to do what the center button did in the previous lab. When you push the dpad to the right position, the file that is selected should be displayed.

- Reassign the center button to do the following:
    - Take a photo with the Pi Z2W's camera
  
    - Save the raw data received from the capture to a `.bmp` file. The files must be named `doorbell-<n>.bmp` where `<n>` is replaced by the current number of pictures taken.
    
    - Your code should save up to 5 `.bmp` images. If a 6th image is taken, `doorbell-1.bmp` should be overwritten, if a 7th is taken `doorbell-2.bmp` should be overwritten, etc.

    - Update your fileviewer code to include the new bitmap files that you save.

    - Show your most recent photo for 5 seconds before you return to the file viewer.

    - All other functionality should stay the same.

## Submission

- Answer the questions in the `README.md`.

- To pass off to a TA, demonstrate your doorbell running your program that fulfills all of the requirements outlined above.

- To successfully submit your lab, you will need to follow the instructions in the [Lab Setup]({{ site.baseurl }}/lab-setup) page, especially the **Committing and Pushing Files** section.


## Explore More!

- [`fwrite()` in C](https://www.tutorialspoint.com/c_standard_library/c_function_fwrite.htm)

- [Memory Allocation in C](https://www.geeksforgeeks.org/dynamic-memory-allocation-in-c-using-malloc-calloc-free-and-realloc/)

- [Connect Camera to Raspberry Pi](https://www.arducam.com/raspberry-pi-camera-pinout/)
