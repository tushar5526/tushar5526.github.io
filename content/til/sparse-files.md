---
title: Creating files bigger than available disk space!
date: "2024-04-06T16:34:25+05:30"
type: "post"
description: "Sample Blog - Setting up a minimalistic blogging/personal website"
in_search_index: true
tags: ["website", "blogging", "portfolio"]
---

### spoiler: sparse files / holes in a file

I was recently going through the book *The Linux Programming Interface* and came across the concept of sparse files. Files are stored on multiple blocks on a disk and sparse files is a smart optimisation that skips allocating disk block if there is no data to be stored. 

You can create holes in your files by moving the file pointer `n` bytes ahead than the actual size and then write some random data to it.

```c
#include <stdio.h>
#include <fcntl.h>
#include <stdlib.h>

int main()
{
    int fd = open("test.txt", O_RDWR | O_CREAT, 0644);
    int val = lseek(fd, 999999999999, SEEK_END);
    if (val == -1)
    {
        printf("error in performing lseek");
        return 0;
    }
    char buffer[7] = "Random bytes to write";
    write(fd, &buffer, 6);
    return 0;
}
````

Compile and run the program. You can see the increased file size of the sparse file. 

```sh
gcc sparse-files-generator.c && ./a.out && ls -lhr
```

> You can also run `ls -s` to see the number of blocks allocated to a file.
