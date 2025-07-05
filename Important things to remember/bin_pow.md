Extremely useful function because the pow() function in c++ is not accurate as it calculates using floating point values. This bin_pow function is also extremely fast. Built on the principle of [[binary exponentiation]]

```
long long bin_pow(long long a, long long b) {
	if(b == 0) return 1;
	long long res = bin_pow(a, b / 2);
	if(b % 2) {
		return res * res * a;
	}
	else return res * res;
}
```

^d34bd0

