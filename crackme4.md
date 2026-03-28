**Objective: find a password using basic reverse engineering skills**

**Main function**
```c
undefined8 main(int param_1,undefined8 *param_2)

{
   if (param_1 == 2) {
      compare_pwd(param_2[1]);
   }
   else {
      printf("Usage : %s password\nThis time the string is hidden and we used strcmp\n",*param_2);
   }
   return 0;
}

int param_1 -> int argc (arguments count)
undefined8 *param_2 -> char *argv[] (arguments array)
```

The code shows all the function does is to check if the second argument (first one is a name of the program while executing with CLI) is equal to the password using compare_pwd function
compare_pwd() looks that way

```c
void compare_pwd(char *param_1)

{
   int iVar1;
   long in_FS_OFFSET;
   char local_28 [24];
   long local_10;
   
   local_10 = *(long *)(in_FS_OFFSET + 0x28);
   builtin_strncpy(local_28,"I]{I\x14V\x17{WAGQV\x17{TS@",0x13);
   get_pwd(local_28);
   iVar1 = strcmp(local_28,param_1);
   if (iVar1 == 0) {
      puts("password OK");
   }
   else {
      printf("password \"%s\" not OK\n",param_1);
   }
   if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                               /* WARNING: Subroutine does not return */
      __stack_chk_fail();
   }
   return;
}
```
---
Here the `strncpy` function copies `0x13` (dec: 19) chars of the string provided as a second argument (so it does copy the entire string) to `char local_28[24]`. Next the `get_pwd()` function is called with this char array passed as an argument. Here's how it looks like:

String: `I` `]` `{` `I` `\x14` `V` `\x17` `{` `W` `A` `G` `Q` `V` `\x17` `{` `T` `S` `@`<br>
Hex: `49 5D 7B 49 14 56 17 7B 57 41 47 51 56 17 7B 54 53 40 `

```c
if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
    __stack_chk_fail();
}
```
This is a stack canary protection and is not relevant to the password logic.

```c
void get_pwd(long param_1) // param_1 = 49 5D 7B 49 14 56 17 7B 57 41 47 51 56 17 7B 54 53 40 
{
   undefined4 local_c;
   
   local_c = -1;
   while (local_c = local_c + 1, *(char *)(param_1 + local_c) != '\0') {
      *(byte *)(local_c + param_1) = *(byte *)(param_1 + local_c) ^ 0x24;
   }
   return;
}
```

All it does is a XOR operation on every value from the array with 0x24 value<br>
XOR is a reversible operation: `A ^ B ^ B = A`<br>
This means the same operation is used for both encoding and decoding.
I've scripted a program to perform XOR operation on every value with 0x24 and print the password.

```cpp
int main() {
    unsigned char data[] = {
    0x49, 0x5D, 0x7B, 0x49, 0x14, 0x56, 0x17, 0x7B,
    0x57, 0x41, 0x47, 0x51, 0x56, 0x17, 0x7B, 0x54,
    0x53, 0x40
    };

    for (const auto& val : data) {
        cout << static_cast<char>(val ^ 0x24);
    }

    return 0;
}
```
Password: `my_m0r3_secur3_pwd`





