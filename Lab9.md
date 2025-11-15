**PROGRAM 9: RSA ALGORITHM (ENCRYPTION & DECRYPTION)**

---

### **RSADemo.java**

```java
import java.util.*;

public class RSADemo {

    public static void main(String[] args) {
        String msg;
        int pt[] = new int[100];
        int ct[] = new int[100];
        int z, n, d, e, p, q, mlen;

        Scanner in = new Scanner(System.in);

        do {
            System.out.println("Enter the two large prime numbers for p and q");
            p = in.nextInt();
            q = in.nextInt();
        } while (prime(p) == 0 || prime(q) == 0);

        n = p * q;
        z = (p - 1) * (q - 1);

        System.out.println("Value of n" + n + "\nValue of z is:" + z);

        for (e = 2; e < z; e++) {
            if (gcd(e, z) == 1)
                break;
        }

        System.out.println("Encryption key e is " + e);
        System.out.println("Public key is (e,n):" + e + "," + n);

        for (d = 2; d < z; d++) {
            if ((e * d) % z == 1)
                break;
        }

        System.out.println("Decryption key d is : " + d);
        System.out.println("Private key is (d,n)=>" + d + "," + n);

        in.nextLine();

        System.out.println("Enter the message for encryption");
        msg = in.nextLine();

        mlen = msg.length();

        for (int i = 0; i < mlen; i++)
            pt[i] = msg.charAt(i);

        System.out.println("ASCII Values of PT array is");
        for (int i = 0; i < mlen; i++)
            System.out.println(pt[i]);

        System.out.println("Encryption: CipherText Obtained:");
        for (int i = 0; i < mlen; i++)
            ct[i] = mult(pt[i], e, n);

        for (int i = 0; i < mlen; i++)
            System.out.print(ct[i] + "\t");

        System.out.println("\nDecryption: PlainText Obtained:");
        for (int i = 0; i < mlen; i++)
            pt[i] = mult(ct[i], d, n);

        for (int i = 0; i < mlen; i++)
            System.out.println(pt[i] + ":" + (char) pt[i]);
    }

    public static int gcd(int x, int y) {
        if (y == 0) return x;
        else return gcd(y, x % y);
    }

    public static int prime(int num) {
        int i;
        for (i = 2; i <= num / 2; i++) {
            if (num % i == 0)
                return 0;
        }
        return 1;
    }

    public static int mult(int base, int exp, int n) {
        int res = 1, j;
        for (j = 1; j <= exp; j++)
            res = ((res * base) % n);
        return res;
    }
}
```

---

### **OUTPUT (RUN 1)**

```
Enter the two large prime numbers for p and q
19
23
Value of n437
Value of z is:396
Encryption key e is 5
Public key is (e,n):5,437
Decryption key d is :317
Private key is (d,n)=>317,437
Enter the message for encryption
smvitmbantakal
ASCII Values of PT array is
115
109
118
105
116
109
32
98
97
110
116
97
107
97
108
Encryption: CipherText Obtained:
115	67	36	326	70	67	261	186	89	325	70	89	122	89	52
Decryption: PlainText Obtained:
115:s
109:m
118:v
105:i
116:t
109:m
32: 
98:b
97:a
110:n
116:t
97:a
107:k
97:a
108:l
```

---

### **OUTPUT (RUN 2)**

```
Enter the two large prime numbers for p and q
23
29
Value of n667
Value of z is:616
Encryption key e is 3
Public key is (e,n):3,667
Decryption key d is :411
Private key is (d,n)=>411,667
Enter the message for encryption
!@#$%
ASCII Values of PT array is
33
64
35
36
37
Encryption: CipherText Obtained:
586	13	187	633	628
Decryption: PlainText Obtained:
33:!
64:@
35:#
36:$
37:%
```
