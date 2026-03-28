**Objective: Analyze the binary for the easy password**

I started my analysis with a main function:

```c
undefined8 main(int param_1,undefined8 *param_2)
{
  if (param_1 == 2) {
    compare_pwd(param_2[1]);
  }
  else {
    printf("Usage : %s password\nGood luck, read the source\n",*param_2);
  }
  return 0;
```

Function `compare_pwd()` is called when one argument except the program name is passed before executing

```c
void compare_pwd(undefined8 param_1)
{
  int iVar1;
  
  iVar1 = my_secure_test(param_1);
  if (iVar1 == 0) {
    puts("password OK");
  }
  else {
    printf("password \"%s\" not OK\n",param_1);
  }
  return;
}
```

The code checks whether another function `my_secure_test()` returned `0` in order to accept the password

```c
undefined8 my_secure_test(char *param_1)
{
  undefined8 uVar1;
  
  if ((*param_1 == '\0') || (*param_1 != '1')) {
    uVar1 = 0xffffffff;
  }
  else if ((param_1[1] == '\0') || (param_1[1] != '3')) {
    uVar1 = 0xffffffff;
  }
  else if ((param_1[2] == '\0') || (param_1[2] != '3')) {
    uVar1 = 0xffffffff;
  }
  else if ((param_1[3] == '\0') || (param_1[3] != '7')) {
    uVar1 = 0xffffffff;
  }
  else if ((param_1[4] == '\0') || (param_1[4] != '_')) {
    uVar1 = 0xffffffff;
  }
  else if ((param_1[5] == '\0') || (param_1[5] != 'p')) {
    uVar1 = 0xffffffff;
  }
  else if ((param_1[6] == '\0') || (param_1[6] != 'w')) {
    uVar1 = 0xffffffff;
  }
  else if ((param_1[7] == '\0') || (param_1[7] != 'd')) {
    uVar1 = 0xffffffff;
  }
  else if (param_1[8] == '\0') {
    uVar1 = 0;
  }
  else {
    uVar1 = 0xffffffff;
  }
  return uVar1;
}
```

In this function there is a set of if/else if statements which check whether the password provided by user is correct by comparing user input relevant character by character with these hard-coded. In case of providing proper password function returns `0`, otherwise `0xffffffff` is returned.

Password: `1337_pwd`
