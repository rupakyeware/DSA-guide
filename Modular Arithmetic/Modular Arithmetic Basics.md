In any [[DSA]] question, when we are asked to take a MOD of the result, certain rules have to be followed. They are as follows-

1. Addition
```
(a + b) % MOD = ((a % MOD) + (b % MOD)) % MOD
```

2. Multiplication
```
(a * b) % MOD = ((a % MOD) * (b % MOD)) % MOD
```

3. Subtraction
```
(a - b) % MOD = (((a % MOD) - (b % MOD)) + MOD) % MOD
```

4. Division
```
(a / b) % MOD = ((a % MOD) * (bin_pow(b, MOD-2) % MOD)) % MOD
```