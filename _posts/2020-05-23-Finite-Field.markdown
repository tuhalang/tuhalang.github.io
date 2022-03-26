---
layout: post
title: Finite Field
categories: [Bitcoin, Math, Crypto]
description: Elliptic Curve
keywords: Elliptic, Cryptography, Finite, Field
---

I. Định nghĩa nhóm
- Nhóm là một tập hợp G và phép toán 2 ngôi * (G, *) thỏa mãn các tính chất sau:
    
    + Tính đóng (closure): Với mọi a, b thuộc G thì ta có a * b thuộc G.
    + Tính kết hợp (Associativity): với mọi a, b, c thuộc G ta có:
    ```(a * b)* c = a * (b * c)```
    + Phần tử đơn vị (identity element): tồn tại một phần tử đơn vị e thuộc G thỏa mãn ```e * a = a * e = a``` với mọi a thuộc G. Nếu tồn tại phần tử đơn vị là duy nhất.
    + Phần tử nghịch đảo (inverse element):với mỗi a thuộc G, tồn tại b thuộc G thỏa mãn ```a * b = b * a = e```

II. Định nghiã trường hữu hạn

1. Finite Field (Trường hữu hạn)
- Trường hữu hạn là một ***tập hữu hạn*** các số (tập F) và hai phép toán cộng (+) và nhân (*) thỏa mãn các tính chất sau: 

    + Các phần tử của F tạo thành một ***nhóm*** với phép toán + với phần tử đơn vị là 0
    + Các phần tử của F ngoại trừ 0 tạo thành một ***nhóm*** với phép toán * với phần tử đơn vị là 1
    + Các phần tử của F cùng với hai phép toán + và * thỏa mãn luật phân phối, tức là:
    ``` 
    a * (b + c) = (a * b) + (a * c) với mọi a, b, c thuộc F.
    ```

2. Finite Set (Tập hữu hạn)
- Nếu kích thước của tập là p thì các phần tử của tập hợp đó là: 0, 1, 2, ..., p-1.
```
Fp = {0, 1, 2, 3, 4, 5, ..., p-1}
```
- Chú ý: Kích thước của tập hợp luôn lớn hơn 1 và là một số nguyên tố .

3. Phép toán lấy module

- Để đảm báo tính đóng của trường hữu hạn khi thực hiện các phép toán cộng, trừ, nhân và chia thì chúng ta thực hiện các phép toán module
- Chúng ta định nghĩa phép cộng trong một tập hữu hạn sử dụng phép toán module như sau:
    + Giả sử F5 = {0, 1, 2, 3, 4} thì
    + 1 + 3 = 4 mod 5
    + 3 + 4 = 2 mod 5
    + ...
- Tương tự đối với các phép toán trừ và nhân.

4. Định lý fermat nhỏ 
- Nếu p là một số nguyên tố thì:

     ```n ^ (p-1) = 1 mod p```
- Từ định lý fermat nhỏ có thể giúp chúng ta dễ dàng thực hiện phép chia 
- Giả sử ta muốn tính
    
    ```a/b mod p```

    ```Ta có: a/b = a * b^(-1)```

    ```Mà b^(p-1) = 1 => b^(-1) = b^(-1) * b^(p-1) = b^(p-2)```

    ```Vậy a/b = a * b^(p-2)```

III. Xây dựng Finite Field với Python

```py
import unittest


class FieldElement:
    def __init__(self, num, prime):
        if num >= prime or num < 0:
            error = 'Num {} not in field range 0 to {}'.format(num, prime - 1)
            raise ValueError(error)
        self.num = num
        self.prime = prime

    def __repr__(self):
        return 'FieldElement_{}({})'.format(self.prime, self.num)

    def __eq__(self, other):
        if other is None:
            return False
        return self.num == other.num and self.prime == other.prime

    def __ne__(self, other):
        if other is None:
            return True
        return self.num != other.num or self.prime != other.prime

    def __add__(self, other):
        if self.prime != other.prime:
            raise TypeError('Cannot add two numbers in different Fields')
        num = (self.num + other.num) % self.prime
        return self.__class__(num, self.prime)

    def __sub__(self, other):
        if self.prime != other.prime:
            raise TypeError('Cannot add two numbers in different Fields')
        num = (self.num - other.num) % self.prime
        return self.__class__(num, self.prime)

    def __mul__(self, other):
        if self.prime != other.prime:
            raise TypeError('Cannot add two numbers in different Fields')
        num = (self.num * other.num) % self.prime
        return self.__class__(num, self.prime)

    def __pow__(self, exponent):
        n = exponent % (self.prime - 1)
        num = pow(self.num, n, self.prime)
        return self.__class__(num, self.prime)

    def __truediv__(self, other):
        if self.prime != other.prime:
            raise TypeError('Cannot divide two numbers in different Fields')
        num = pow(other.num, self.prime-2, self.prime) * self.num % self.prime
        return self.__class__(num, self.prime)


class FieldElementTest(unittest.TestCase):

    def test_ne(self):
        a = FieldElement(2, 31)
        b = FieldElement(2, 31)
        c = FieldElement(15, 31)
        self.assertEqual(a, b)
        self.assertTrue(a != c)
        self.assertFalse(a != b)

    def test_add(self):
        a = FieldElement(2, 31)
        b = FieldElement(15, 31)
        self.assertEqual(a + b, FieldElement(17, 31))
        a = FieldElement(17, 31)
        b = FieldElement(21, 31)
        self.assertEqual(a + b, FieldElement(7, 31))

    def test_sub(self):
        a = FieldElement(29, 31)
        b = FieldElement(4, 31)
        self.assertEqual(a - b, FieldElement(25, 31))
        a = FieldElement(15, 31)
        b = FieldElement(30, 31)
        self.assertEqual(a - b, FieldElement(16, 31))

    def test_mul(self):
        a = FieldElement(24, 31)
        b = FieldElement(19, 31)
        self.assertEqual(a * b, FieldElement(22, 31))

    def test_pow(self):
        a = FieldElement(17, 31)
        self.assertEqual(a**3, FieldElement(15, 31))
        a = FieldElement(5, 31)
        b = FieldElement(18, 31)
        self.assertEqual(a**5 * b, FieldElement(16, 31))

    def test_div(self):
        a = FieldElement(3, 31)
        b = FieldElement(24, 31)
        self.assertEqual(a / b, FieldElement(4, 31))
        a = FieldElement(17, 31)
        self.assertEqual(a**-3, FieldElement(29, 31))
        a = FieldElement(4, 31)
        b = FieldElement(11, 31)
        self.assertEqual(a**-4 * b, FieldElement(13, 31))


unittest.main()

```


