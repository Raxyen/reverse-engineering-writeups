I've just written a quick cpp code to break the encryption. Key: SNEAKY

```cpp
#include <iostream>

using namespace std;

int hex_to_dec(char c1, char c2) {
	char i1 = 0, i2 = 0;
	if (c1 >= '0' && c1 <= '9' && c2 >= '0' && c2 <= '9') {
		i1 = c1 - 48;
		i2 = c2 - 48;
	}
	else if (c1 >= 'a' && c1 <= 'f' && c2 >= '0' && c2 <= '9') {
		i1 = c1 - 87;
		i2 = c2 - 48;
	}
	else if (c1 >= '0' && c1 <= '9' && c2 >= 'a' && c2 <= 'f') {
		i1 = c1 - 48;
		i2 = c2 - 87;
	}
	else if (c1 >= 'a' && c1 <= 'f' && c2 >= 'a' && c2 <= 'f') {
		i1 = c1 - 87;
		i2 = c2 - 87;
	}
	return i1 * 16 + i2;
}

int main() {
	string s = "1c1c01041963730f31352a3a386e24356b3d32392b6f6b0d323c22243f63731a0d0c302d3b2b1a292a3a38282c2f222d2a112d282c31202d2d2e24352e60";
	int c[62];
	for (size_t i = 0; i < s.size(); i += 2) {
		c[(int)(i / 2)] = hex_to_dec(s[i], s[i + 1]);
	}

	for (size_t i = 0; i < 63; i++) {
		switch (i % 6) {
		case 0:
			cout << (char)(c[i] ^ 83);
			break;
		case 1:
			cout << (char)(c[i] ^ 78);
			break;
		case 2:
			cout << (char)(c[i] ^ 69);
			break;
		case 3:
			cout << (char)(c[i] ^ 65);
			break;
		case 4:
			cout << (char)(c[i] ^ 75);
			break;
		case 5:
			cout << (char)(c[i] ^ 89);
			break;
		}
	}
	return 0;
}
```
