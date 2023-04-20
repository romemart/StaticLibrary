#### Statically linked or Static Library


Create a header file called mymath.h containing:
```c
int add(int a, int b);
int sub(int a, int b);
int mult(int a, int b);
int divi(int a, int b);
```
There, will be all declarations used in main process, where the implementations of these functions will be defined in the static library ".a" extension.

Create our main process called mathDemo.c containing:
```c
#include <mymath.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, const char* argv[])
{
  int x, y;
  printf("Enter two numbers\n");
  scanf("%d%d",&x,&y);

  printf("\n%d + %d = %d", x, y, add(x, y));
  printf("\n%d - %d = %d", x, y, sub(x, y));
  printf("\n%d * %d = %d", x, y, mult(x, y));

  if(y==0){
    printf("\nDenominator is zero so can't perform division\n");
    exit(0);
  }else{
    printf("\n%d / %d = %d\n", x, y, divi(x, y));
    return 0;
  }
}
```
And on the other hand.
Create a folder named static where all the static library implementation files will be located:

`$ mkdir Static`

create a file `add.c` containing:
```c
int add(int a, int b){
    return (a+b);
}
```
create a file `sub.c` containing:
```c
int sub(int a, int b){
    return (a-b);
}
```
create a file `mul.c` containing:
```c
int mult(int a, int b){
    return (a*b);
}
```
create a file `div.c` containing:
```c
int divi(int a, int b){
    return (a/b);
}
```
Our main process is called mathDemo 
```bash
$ tree
.
├── mathDemo.c
├── mymath.h
└── Static
    ├── add.c
    ├── div.c
    ├── mul.c
    └── sub.c
```
Inside the folder called "Static". Create object files:

`$ gcc -c add.c sub.c mul.c div.c`

Create a static library called libmymath.a

`$ ar rs libmymath.a add.o sub.o mul.o div.o`

then remove the object files, as they're no longer required.

`$ rm *.o`

```bash
$ ls
add.c  div.c  libmymath.a  mul.c  sub.c
```
To create a statically linked application:

Inside the main folder

Create an object file called mathDemo.o from mathDemo.c:
If the header file is included between quotes `"headerFile"`:

`$ gcc -c mathDemo.c -o mathDemo.o` 

or if it is included using brackets `<headerFile>` instead:

`$ gcc -I . -c mathDemo.c`

The `-I` option tells GCC to search for header files listed after it. In this case, you're specifying the current directory, represented by a single dot `.`

Following step

Link mathDemo.o with libmymath.a to create the final executable. 
There are two ways to express this to GCC.
Knowing: -L{path to file containing library} -l${library name}

`$ gcc -static -o mathDemo mathDemo.o ./Static/libmymath.a`

or

`$ gcc -static -o mathDemo -L./Static/ mathDemo.o -lmymath` 

Then remove the object files, as they're no longer required.

`$ rm *.o`

Analyzing the result
```bash
$ file mathDemo
mathDemo: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), 
statically linked, BuildID[sha1]=7f0e701f1c1dd096efebb6faaa927641e8b5633a, 
for GNU/Linux 3.2.0, not stripped
```