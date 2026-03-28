**Objective: obtain the flag**

```c
undefined4 FUN_080484f4(int param_1,undefined4 *param_2)
{
  char *__s;
  size_t sVar1;
  char *__s_00;
  int iVar2;
  
  if (param_1 == 2) {
    __s = (char *)param_2[1];
    sVar1 = strlen(__s);
    __s_00 = malloc(sVar1 * 2);
    if (__s_00 == (char *)0x0) {
      fwrite("malloc failed\n",0xe,1,stderr);
    }
    else {
      sVar1 = strlen(__s);
      FUN_080486b0(__s,__s_00,sVar1,0);
      sVar1 = strlen(__s_00);
      if ((sVar1 == 0x40) &&
         (iVar2 = strcmp(__s_00,"ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ=="),
         iVar2 == 0)) {
        puts("Correct password!");
        return 0;
      }
      puts("Come on, even my aunt Mildred got this one!");
    }
  }
  else {
    fprintf(stderr,"Usage: %s PASSWORD\n",*param_2);
  }
  return 0xffffffff;

```
After decompling the ELF with Ghidra we get 4 functions containing some real code. We can ignore the rest. Above there's one of those functions containing a string being a base64 encoded flag we need to find in this task. Remaining 3 functions are responsible for base64 encoding but we don't need to analyze them. All we have to know is how base64 encoded strings look like.

Base64 flag: `ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ==`<br>
Decoded flag: f0r_y0ur_5ec0nd_le55on_unbase64_4ll_7h3_7h1ng5
