**PROGRAM 4: ERROR DETECTION USING CRC–CCITT (16-BIT)**
**(Copy–paste exactly. Code corrected for clean compilation)**

---



**CRCDemo.java**

```java
import java.util.Scanner;

public class CRCDemo {
    static String msg;
    static String genPoly = "10001000000100001"; // CRC-CCITT 16-bit
    static char t[] = new char[128];   // Message + checksum
    static char cs[] = new char[128];  // Checksum temp
    static char g[] = new char[128];   // Generator polynomial
    static int mlen, glen, x, c, flag = 0, test;

    public static void main(String[] args) {

        Scanner in = new Scanner(System.in);
        System.out.println("Enter the message to be transferred:");
        msg = in.nextLine();
        mlen = msg.length();

        for (int i = 0; i < mlen; i++)
            t[i] = msg.charAt(i);

        System.out.println("Predefined Generator Polynomial is: " + genPoly);

        g = genPoly.toCharArray();
        glen = genPoly.length();

        // append zeros
        for (x = mlen; x < (mlen + glen - 1); x++)
            t[x] = '0';

        System.out.println("Zero extended message is: " + new String(t).trim());

        crc(); // checksum

        System.out.println("CheckSum is: " + new String(cs, 0, glen - 1));

        // append checksum
        for (x = mlen; x < mlen + glen - 1; x++)
            t[x] = cs[x - mlen];

        System.out.println("Final codeword generated is: " + new String(t).trim());

        System.out.println("\nTest Error detection 1(yes) 0(no)? : ");
        test = in.nextInt();

        if (test == 1) {
            System.out.println("Enter position where error is to be inserted:");
            x = in.nextInt();
            t[x] = (t[x] == '0') ? '1' : '0';
            System.out.println("Erroneous data: " + new String(t).trim());
        }

        crc(); // receiver side

        flag = 0;
        for (x = 0; x < glen - 1; x++) {
            if (cs[x] == '1') {
                flag = 1;
                break;
            }
        }

        if (flag == 1)
            System.out.println("Error was detected during transfer");
        else
            System.out.println("No Error Detected during transfer");
    }

    public static void crc() {
        for (x = 0; x < glen; x++)
            cs[x] = t[x];

        do {
            if (cs[0] == '1')
                xor();

            for (c = 0; c < glen - 1; c++)
                cs[c] = cs[c + 1];

            cs[c] = t[x++];
        } while (x <= mlen + glen - 1);
    }

    public static void xor() {
        for (c = 1; c < glen; c++)
            cs[c] = (cs[c] == g[c]) ? '0' : '1';
    }
}
```

---

### ✅ **OUTPUT (RUN 1)**

```
Enter the message to be transferred:
10011
Predefined Generator Polynomial is: 10001000000100001
Zero extended message is: 100110000000000000000
CheckSum is: 0010001001010010
Final codeword generated is: 100110010001001010010

Test Error detection 1(yes) 0(no)? :
1
Enter position where error is to be inserted:
4
Erroneous data: 100100010001001010010
Error was detected during transfer
```

---

### ✅ **OUTPUT (RUN 2)**

```
Enter the message to be transferred:
10011
Predefined Generator Polynomial is: 10001000000100001
Zero extended message is: 100110000000000000000
CheckSum is: 0010001001010010
Final codeword generated is: 100110010001001010010

Test Error detection 1(yes) 0(no)? :
0
No Error Detected during transfer
```

---

