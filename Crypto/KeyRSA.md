# KeyRSA

> nc 103.245.249.76 49160

Source code 

```Python
from Crypto.Util.number import bytes_to_long, getPrime

flag = b'<REDACTED>'

p, q = getPrime(512), getPrime(512)
n = p * q
e = 65537

msg = bytes_to_long(flag)

assert n > msg
ct = pow(msg,e,n)

phi = (p-1) * (q-1)
d = pow(e,-1,phi)

yn = input(b'Do you want to try your own key? [ y/n ] : ) ')
if yn == 'y':
    print('\nSure, here are your keys \n')
    print(f'\ne = {e}')
    print(f'\nn = {n}')
    print(f'\nx = {p % (n // 2)}')

    user_d = int(input("\nEnter your key : \n"))
    if user_d != d:
        if pow(ct,user_d,n) == pow(ct,d,n):
            print(flag)
        else:
            print('Better luck next time !!')
            exit(0)
    else:
        print("Sorry you can't use my key, or maybe our keys were similar this time, try again !!")
        exit(0)
else:
    print('See you next time')
    exit(0)

```

Từ source code, chúng ta có thể lấy được vài thông tin có giá trị như sau

```python
print(f'\nx = {p % (n // 2)}')
```

vì p và q là các số nguyên tố nhỏ hơn bằng 512  
=> `2 <= p,q`  
=> `2 <= p.q`
=> `n//2 >= p,q` 

vậy `p% (n//2)` = `p` hay `x` = `p`.  
Từ đây chúng ta tính được `q`, `phi` và `d`.  

Để lấy được flag, chúng ta phải truyền vào `user_d` sao cho `user_d != d && pow(ct,user_d,n) == pow(ct,d,n)`

Ta biết pow(a,b,c) == a**b%c.  
=> ct**user_d%n == ct**d%n  
<=> ct**user_d ≡ ct**d (mod n)  
<=> (m**e)**user_d ≡ (m**e)**d (mod n)  
<=> m**(e\*user_d) ≡ m**ed (mod n)  
<=> e\*user_d ≡ e\*d ≡ 1 (mod n)  
Ta có ed ≡ (ed)^2 ≡ 1 (mod n)
=> user_d = e(d^2) 

Bằng cách tính nhanh giá trị của user_d trước khi timeout, chúng ta sẽ có được flag.

#### Flag: FPTUHacking{Y0u_kn0w_t0_CR34t3_y0ur_0wn_k3y!!!}
