**PROGRAM 10: LEAKY BUCKET ALGORITHM**

---

### **leaky.java**

```java
import java.util.Scanner;

public class leaky {
    public static void main(String[] args) {

        int i;
        int a[] = new int[20];
        int buck_rem = 0, sent, recv;

        Scanner in = new Scanner(System.in);

        System.out.println("Enter the number of packets:");
        int n = in.nextInt();

        System.out.println("Enter the bucket capacity:");
        int buck_cap = in.nextInt();

        System.out.println("Enter the output rate:");
        int rate = in.nextInt();

        System.out.println("Enter the size of packets");
        for (i = 1; i <= n; i++)
            a[i] = in.nextInt();

        System.out.println("CLOCK\t PACKET SIZE\t ACCEPT\t SENT \tREMAINING");

        for (i = 1; i <= n; i++) {

            if (a[i] != 0) {

                if (buck_rem + a[i] > buck_cap)
                    recv = -1;
                else {
                    recv = a[i];
                    buck_rem += a[i];
                }

            } else
                recv = 0;

            if (buck_rem != 0) {
                if (buck_rem < rate) {
                    sent = buck_rem;
                    buck_rem = 0;
                } else {
                    sent = rate;
                    buck_rem = buck_rem - rate;
                }
            } else
                sent = 0;

            if (recv == -1)
                System.out.println(i + "\t\t" + a[i] + "\tdropped\t" + sent + "\t" + buck_rem);
            else
                System.out.println(i + "\t\t" + a[i] + "\t\t" + recv + "\t" + sent + "\t" + buck_rem);
        }
    }
}
```

---

### **OUTPUT**

```
Enter the number of packets
5
Enter the bucket capacity
4
Enter the output rate
3
Enter the size of packets
2 4 1 5 3

CLOCK   PACKET SIZE     ACCEPT  SENT    REMAINING
1               2               2       2       0
2               4       dropped 3       1
3               1               1       1       0
4               5       dropped 0       0
5               3               3       3       0
```
