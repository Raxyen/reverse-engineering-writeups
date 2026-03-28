**Objective: Find the input to get output Good game**

In this disassembled ELF file there's an only one relevant function (`main`) in context of finding the correct input 

**Main function:**
```c
undefined8 main(void)
{
  int iVar1;
  long in_FS_OFFSET;
  undefined1 local_58 [32];
  undefined1 local_38;
  undefined1 local_37;
  undefined1 local_36;
  undefined1 local_35;
  undefined1 local_34;
  undefined1 local_33;
  undefined1 local_32;
  undefined1 local_31;
  undefined1 local_30;
  undefined1 local_2f;
  undefined1 local_2e;
  undefined1 local_2d;
  undefined1 local_2c;
  undefined1 local_2b;
  undefined1 local_2a;
  undefined1 local_29;
  undefined1 local_28;
  undefined1 local_27;
  undefined1 local_26;
  undefined1 local_25;
  undefined1 local_24;
  undefined1 local_23;
  undefined1 local_22;
  undefined1 local_21;
  undefined1 local_20;
  undefined1 local_1f;
  undefined1 local_1e;
  undefined1 local_1d;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_38 = 0x4f;
  local_37 = 0x66;
  local_36 = 100;
  local_35 = 0x6c;
  local_34 = 0x44;
  local_33 = 0x53;
  local_32 = 0x41;
  local_31 = 0x7c;
  local_30 = 0x33;
  local_2f = 0x74;
  local_2e = 0x58;
  local_2d = 0x62;
  local_2c = 0x33;
  local_2b = 0x32;
  local_2a = 0x7e;
  local_29 = 0x58;
  local_28 = 0x33;
  local_27 = 0x74;
  local_26 = 0x58;
  local_25 = 0x40;
  local_24 = 0x73;
  local_23 = 0x58;
  local_22 = 0x60;
  local_21 = 0x34;
  local_20 = 0x74;
  local_1f = 0x58;
  local_1e = 0x74;
  local_1d = 0x7a;
  puts("Enter your input:");
  __isoc99_scanf(&DAT_00400966,local_58);
  iVar1 = strcmp_(local_58,&local_38);
  if (iVar1 == 0) {
    puts("Good game");
  }
  else {
    puts("Always dig deeper");
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```

As we can see `_iosc99_scanf()` (thus `scanf` function) asks user for an input and if the input is equal to `&local_38` we are given the expected output (Good game). It's clear that `local_38` is a first element of the array containing all `local_XX` char variables (`XX` stands for `38`, `37`, ... , `1e`, `1d`). All we have to do is to read the values and cast it to chars to print the proper input

```cpp
unsigned char data[] = {
0x4f, 0x66, 100, 0x6c, 0x44, 0x53, 0x41, 0x7c,
0x33, 0x74, 0x58, 0x62, 0x33, 0x32, 0x7e, 0x58,
0x33, 0x74, 0x58, 0x40, 0x73, 0x58, 0x60, 0x34,
0x74, 0x58, 0x74, 0x7a
};

for (const auto& val : data) {
    cout << static_cast<char>(val);
} 
```

Correct input: ```OfdlDSA|3tXb32~X3tX@sX`4tXtz```