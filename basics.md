# C Programming concepts

## array

```
int nums[5] = {10, 20, 30, 40, 50}4
printf("%d\n" nums[2]) // 30
printf("%d\n", *(nums + 2)) // 30, same thing, pointer arithmetic
printf("%d\n", sizeof(nums)) // 20 (5 * sizeof(int))
```

## struct

```
struct Point {
  int x;
  int y;
};

struct Point p = {3, 4};
p.x = 10;

struct Point *pp = &p;
pp->y = 20

struct Rect {
  struct Point top_left;
  struct Point bottom_right;
}

void move_point(struct Point *p, int dx, int dx){
  p->x += dx;
  p->y += dy;
}
```

## memory

    - stack (automatic, freed when function returns)
    - heap (malloc/free, you manage it)
    - static/global storage

```
#include <stdlib.h>
int *make_array(int n) {
  int *arr = malloc(n * sizeof(int));
  if (!arr) return NULL;
  for (int i = 0; i < n; i++) arr[i] = i * i;
  return arr;
}
int main(void) {
  int *a = make_array(10);
  printf("%d\n", a[5])
  free(a);
  a = NULL;
}
```

## pointers

```
int x = 5;
int *px = &x;
*px = 10;
printf("%d\n", x)

int **ppx = &px;
**ppx = 20;

int add(int a, int b) { return a + b; }
int (*op)(int, int) = add;
printf("%d\n", op(2, 3)); // 5

```

# debugging

## printf() and assert()

```
#include <assert.h>

int divide(int a, int b) {
  assert(b != 0)
  return a / b
}
```

## inspect syscalls with strace

```
strace ./a.out
strace -f ./a.out // follow forked child process too
strace -e trace=open,read ./a.out // filter to specific calls
strace -o out.log ./a.out // write trace to a file instead of stderr
```

## inspect live programs or core dumps with gdb, show stack traces and etc.

```
g++ -g -o a.out main.cpp // -g keeps debug symbols
gdb ./a.out
```

```
ulimit -c unlimited // allow core dumps
./a.out // let it crash
gdb ./a.out core // load the core dump
(gdb) backtrace // see exactly where and how it crashed
(gdb) frame 2 // jump to a specific stack frame
(gdb) print somevar // inspect variables at that point in the crash
```
