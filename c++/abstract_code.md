**不使用加法实现加法**

```CPP
int sub(int a, int b) {
    if (!a) return b;
    return sub((a & b) << 1, a ^ b);
}
int main(void) {
    int a, b;
    cin >> a >> b;
    cout << sub(a, b) << endl;
}
```

