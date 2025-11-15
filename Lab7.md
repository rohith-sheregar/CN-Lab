**PROGRAM 7: TCP/IP CLIENT–SERVER FILE TRANSFER**

---

### **CLIENT PROGRAM (SocketClient.java)**

```java
import java.net.*;
import java.io.*;
import java.util.*;

public class SocketClient {
    public static void main(String args[]) throws Exception {
        Scanner in = new Scanner(System.in);

        Socket clientSocket = new Socket("127.0.0.1", 4000);
        System.out.println("****Client side****");

        System.out.println("Enter the filename to transfer");
        String fname = in.nextLine();

        OutputStream ostream = clientSocket.getOutputStream();
        PrintWriter pwrite = new PrintWriter(ostream, true);
        pwrite.println(fname);

        InputStream istream = clientSocket.getInputStream();
        BufferedReader socketRead = new BufferedReader(new InputStreamReader(istream));

        System.out.println("Contents of the file " + fname + " are");
        String str;

        while ((str = socketRead.readLine()) != null) {
            System.out.println(str);
        }

        pwrite.close();
        socketRead.close();
        clientSocket.close();
    }
}
```

---

### **SERVER PROGRAM (SocketServer.java)**

```java
import java.net.*;
import java.io.*;

public class SocketServer {
    public static void main(String args[]) throws Exception {

        ServerSocket servSocket = new ServerSocket(4000);
        System.out.println("****Server Side****");
        System.out.println("Server ready for connection");

        Socket connSock = servSocket.accept();
        System.out.println("Connection is successful and ready for file transfer");

        InputStream istream = connSock.getInputStream();
        BufferedReader fileRead = new BufferedReader(new InputStreamReader(istream));
        String fname = fileRead.readLine();

        File fileName = new File(fname);

        OutputStream ostream = connSock.getOutputStream();
        PrintWriter pwrite = new PrintWriter(ostream, true);

        if (fileName.exists()) {
            BufferedReader contentRead = new BufferedReader(new FileReader(fname));
            System.out.println("Writing file contents to the socket");
            String str;

            while ((str = contentRead.readLine()) != null) {
                pwrite.println(str);
            }
            contentRead.close();
        } else {
            System.out.println("Requested file does not exist");
            pwrite.println("Requested file does not exist at server side");
        }

        connSock.close();
        servSocket.close();
        fileRead.close();
        pwrite.close();
    }
}
```

---

### **HOW TO RUN**

```
SocketServer runs First
(java runs and waits)

open another terminal:
Then run SocketClient
```

Ensure the file (example: Rohith.txt) exists in the same folder.

---

### **OUTPUT**

**Server Side**

```
****Server Side****
Server ready for connection
Connection is successful and ready for file transfer
Writing file Contents to the socket
```

**Client Side**

```
****Client side****
Enter the filename to transfer
rohith.txt
Contents of the file name.txt are
SMVITM
NITK
IIT Madras
Stanford University
```
