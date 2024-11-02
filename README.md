# ACACACACAC
<img width="899" alt="截屏2024-11-02 22 43 09" src="https://github.com/user-attachments/assets/ad1858f7-a53c-44c6-8557-076523040295">
<img width="899" alt="截屏2024-11-02 22 43 17" src="https://github.com/user-attachments/assets/be3588be-4973-41ab-b6bb-8b0f7d6071d0">
<img width="889" alt="截屏2024-11-02 22 44 56" src="https://github.com/user-attachments/assets/50ff06f1-3c5e-4e6c-92ed-beeb6c87512a">
<img width="841" alt="截屏2024-11-02 22 45 20" src="https://github.com/user-attachments/assets/e340487d-0637-4b31-9bd0-dbbc50e0a307">

```
#include <iostream>
#include <vector>

using namespace std;

const long long MOD = 1e9 + 7;
const int MAXN = 2e6 + 5;

vector<long long> factorial(MAXN);

void read(long long &x) {
    x = 0;
    char c = getchar();
    while (c < '0' || c > '9') c = getchar();
    while (c >= '0' && c <= '9') {
        x = x * 10 + c - '0';
        c = getchar();
    }
}

long long quick_power(long long a, long long b) {
    long long result = 1;
    a %= MOD;
    while (b) {
        if (b & 1) result = result * a % MOD;
        a = a * a % MOD;
        b >>= 1;
    }
    return result;
}

long long mod_inverse(long long x) {
    return quick_power(x, MOD - 2);
}

long long comb(long long n, long long k) {
    if (k < 0 || k > n) return 0;
    return factorial[n] * mod_inverse(factorial[k] * factorial[n - k] % MOD) % MOD;
}

void preprocess_factorial() {
    factorial[0] = 1;
    for (int i = 1; i < MAXN; ++i) {
        factorial[i] = factorial[i - 1] * i % MOD;
    }
}

long long calculate_answer(long long n, long long k) {
    long long ans_part1 = factorial[n] * factorial[2 * n - 2 * k] % MOD * factorial[2 * k] % MOD;
    long long ans_part2 = mod_inverse(quick_power(2, n)) * mod_inverse(factorial[k]) % MOD * mod_inverse(factorial[n - k]) % MOD;
    long long ans = ans_part1 * ans_part2 % MOD;

    if (k == 0 || k == n) {
        return ans;
    } else {
        long long extra_part1 = (k + 1) * 4 * k % MOD * factorial[n] % MOD * factorial[2 * (n - k - 1)] % MOD * factorial[2 * (k - 1)] % MOD;
        long long extra_part2 = mod_inverse(quick_power(2, n)) * mod_inverse(factorial[k + 1]) % MOD * mod_inverse(factorial[n - k - 1]) % MOD;
        long long extra = extra_part1 * extra_part2 % MOD;
        return (ans + extra) % MOD;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    preprocess_factorial();

    long long T;
    read(T);
    while (T--) {
        long long n, k;
        read(n);
        read(k);
        long long result = calculate_answer(n, k);
        cout << result << '\n';
    }

    return 0;
}
```
