**Objective: Analyze the binary to get the flag**

The only one function necessary to analyze is this challenge is `giveFlag()`

```c
void giveFlag(void)
{
  int iVar1;
  undefined4 *puVar2;
  undefined4 *puVar3;
  char local_ca [34];
  undefined4 local_a8 [34];
  uint local_20;
  
  puVar2 = &DAT_080488e0;
  puVar3 = local_a8;
  for (iVar1 = 0x22; iVar1 != 0; iVar1 = iVar1 + -1) {
    *puVar3 = *puVar2;
    puVar2 = puVar2 + 1;
    puVar3 = puVar3 + 1;
  }
  memset(local_ca,0x41,0x22);
  for (local_20 = 0; local_20 < 0x22; local_20 = local_20 + 1) {
    local_ca[local_20] = (char)local_a8[local_20] + local_ca[local_20];
  }
  puts(local_ca);
  return;
}
```
---
The flag (which is a 34-character string) was obfuscated using an array of 34 unsigned integers and an 34-chars array filled with value `0x41` (65 in decimal). Above the code there was a pointer variable refering to local_ec array (the one with 34 unsigned ints needed to deobfuscate the flag)

I've found the entire `local_a8` array under the `0x080488e0` memory location using Ghidra

`
25 00 00 00 2b 00 00 00 20 00 00 00 26 00 00 00 3a 00 00 00 2c 00 00 00 34 00 00 00 22 00 00 00 27 00 00 00 1e 00 00 00 31 00 00 00 24 00 00 00 35 00 00 00 24 00 00 00 31 00 00 00 32 00 00 00 28 00 00 00 2d 00 00 00 26 00 00 00 1e 00 00 00 35 00 00 00 24 00 00 00 31 00 00 00 38 00 00 00 1e 00 00 00 28 00 00 00 23 00 00 00 20 00 00 00 1e 00 00 00 36 00 00 00 2e 00 00 00 36 00 00 00 3c 00 00 00 bf ff ff ff
`

I've written down all the non zero bytes. Every value here is 4 bytes because it's an unsigned integer and it's supposed to be a char (which is one byte) after adding value from `local_ca` char array so that's why we see three bytes padding.

Last value `bf ff ff ff` (0xffffffbf) is a `-65` value in unsigned int. Code mentioned above added `65` to each int so `-65 + 65 = 0` (null terminator which is the last char signalizing the end of the string in C)

After all necessary 34 bytes were copied I've made a C++ code to make adding `65` to each value quicker:

```cpp
	ifstream file;
	file.open("C:\\Users\\user\\Desktop\\numbers.txt");
	int n;
	while (file >> n) {
		cout << static_cast<char>(n + 65);
	}
	return 0;
```

Code output: `flag{much_reversing_very_ida_wow}`




