**Objective: Find the super-secret password and use it to obtain the flag**

```c
undefined4 main(int param_1,undefined4 *param_2)
{
  undefined4 uVar1;
  int iVar2;
  
  if (param_1 == 2) {
    iVar2 = strcmp((char *)param_2[1],"super_secret_password");
    if (iVar2 == 0) {
      puts("Access granted.");
      giveFlag();
      uVar1 = 0;
    }
    else {
      puts("Access denied.");
      uVar1 = 1;
    }
  }
  else {
    printf("Usage: %s password\n",*param_2);
    uVar1 = 1;
  }
  return uVar1;
}

void giveFlag(void)

{
  int iVar1;
  undefined4 *puVar2;
  undefined4 *puVar3;
  char local_11f [51];
  undefined4 local_ec [51];
  uint local_20;
  
  puVar2 = &DAT_080486c0;
  puVar3 = local_ec;
  for (iVar1 = 0x33; iVar1 != 0; iVar1 = iVar1 + -1) {
    *puVar3 = *puVar2;
    puVar2 = puVar2 + 1;
    puVar3 = puVar3 + 1;
  }
  memset(local_11f,0x41,0x33);
  for (local_20 = 0; local_20 < 0x33; local_20 = local_20 + 1) {
    local_11f[local_20] = (char)local_ec[local_20] + local_11f[local_20];
  }
  puts(local_11f);
  return;
}
```
---

The password can be identified directly in the main function via `strcmp`:

`iVar2 = strcmp((char *)param_2[1],"super_secret_password");`

Second thing to do was to find the flag and I couldn't find it anywhere in the code. I have found this in `giveFlag()` function:

```c
char local_11f [51];
undefined4 local_ec [51]; // unsigned int array
memset(local_11f,0x41,0x33); // set all bytes to 0x41 (dec 65)
for (local_20 = 0; local_20 < 0x33; local_20 = local_20 + 1) {
    local_11f[local_20] = (char)local_ec[local_20] + local_11f[local_20];
}
```

The flag (which is a 51-character string) was obfuscated using an array of 51 unsigned integers and an 51-chars array filled with value `0x41` (65 in decimal). Above the code there was a pointer variable refering to local_ec array (yeah, that one with 51 unsigned ints needed to deobfuscate the flag)

```c
puVar2 = &DAT_080486c0;
  puVar3 = local_ec;
  for (iVar1 = 0x33; iVar1 != 0; iVar1 = iVar1 + -1) {
    *puVar3 = *puVar2;
    puVar2 = puVar2 + 1;
    puVar3 = puVar3 + 1;
  } 
```

I've found the entire `local_ec` array under the `0x080486c0` memory location using Ghidra

```
                             DAT_080486c0                                    XREF[2]:     giveFlag:08048538(*), 
                                                                                          giveFlag:08048548(R)  
        080486c0 25 00 00 00     undefined4 00000025h local_ec[0]
                             DAT_080486c4                                    XREF[1]:     giveFlag:08048548(R)  
        080486c4 2b 00 00 00     undefined4 0000002Bh local_ec[1]
        080486c8 20              ??         20h        
        080486c9 00              ??         00h
        080486ca 00              ??         00h
        080486cb 00              ??         00h       
        080486cc 26              ??         26h    &  so on...
        080486cd 00              ??         00h

And so on (204 bytes: 51 * 4)...
```

I've written down all the non zero bytes. Every value here is 4 bytes because it's an unsigned integer and it's supposed to be a char (which is one byte) after adding value from `local_11f` char array so that's why we see three bytes padding.

HEX:
`20 26 3a 28 25 1e 28 1e 32 34 21 2c 28 33 1e 33 27 28 32 1e 25 2b 20 26 1e 33 27 24 2d 1e 28 1e 36 28 2b 2b 1e 26 24 33 1e 2f 2e 28 2d 33 32 3c bf ff ff ff`

DEC:
`32 38 58 40 37 30 40 30 50 52 33 44 40 51 30 51 39 40 50 30 37 43 32 38 30 51 39 36 45 30 40 30 54 40 43 43 30 38 36 51 30 47 46 40 45 51 50 60 -65`

`bf ff ff ff` (0xffffffbf) is a `-65` value in unsigned int. Code mentioned above added `65` to each int so `-65 + 65 = 0` (null terminator which is the last char signalizing the end of the string in C)

After all necessary 51 bytes were copied I've made a C++ code to make adding `65` to each value quicker:

```cpp
	ifstream file;
	file.open("C:\\Users\\user\\Desktop\\numbers.txt");
	int n;
	while (file >> n) {
		cout << static_cast<char>(n + 65);
	}
	return 0;
```

Code output: `flag{if_i_submit_this_flag_then_i_will_get_points}`

Here we go!



