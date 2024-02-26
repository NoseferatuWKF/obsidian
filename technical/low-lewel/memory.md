[[jargon|segmented vs paged]]
>there is also a hybrid of segmented and paged memory management

| segmented                                                    | paged                                           |
| ------------------------------------------------------------ | ----------------------------------------------- |
| faster access as entire block of code available to processor | can lead to fragmented process which run slowly |
| fragmentation of free space                                  | makes better use of free space                  |

memory layout
```
-------------------
|    Text Segment  |    // Contains executable code
-------------------
|   Data Segment   |    // Contains static and global variables
-------------------
|    Heap          |    // Dynamically allocated memory
-------------------
|    Stack         |    // Contains local variables and function call stack
-------------------
```