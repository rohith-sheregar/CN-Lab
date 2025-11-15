**PROGRAM 8: DATAGRAM SOCKET CLIENT–SERVER MESSAGE DISPLAY**

---

### **CLIENT PROGRAM (DatagramSocketClient.java)**

```java
import java.net.*;

public class DatagramSocketClient {
    public static void main(String[] args) throws Exception {

        String line = "Connected with Client";

        DatagramSocket clientSocket = new DatagramSocket();
        InetAddress IPAddress = InetAddress.getByName("localhost");

        byte[] sendData = new byte[1024];
        byte[] receiveData = new byte[1024];

        sendData = line.getBytes();

        DatagramPacket sendPacket =
                new DatagramPacket(sendData, sendData.length, IPAddress, 9000);

        clientSocket.send(sendPacket);
        System.out.println("*****Client Display Terminal****");

        while (true) {

            DatagramPacket receivePacket =
                    new DatagramPacket(receiveData, receiveData.length);

            clientSocket.receive(receivePacket);

            String messageReceived = new String(
                    receivePacket.getData(),
                    receivePacket.getOffset(),
                    receivePacket.getLength()
            );

            System.out.println("Message typed at server side is : " + messageReceived);
        }
    }
}
```

---

### **SERVER PROGRAM (DatagramSocketServer.java)**

```java
import java.net.*;
import java.util.*;

public class DatagramSocketServer {
    public static void main(String[] args) throws Exception {

        Scanner in = new Scanner(System.in);

        DatagramSocket serverSocket = new DatagramSocket(9000);

        byte[] receiveData = new byte[1024];
        byte[] sendData = new byte[1024];

        System.out.println("***Server Side***");

        DatagramPacket receivePacket =
                new DatagramPacket(receiveData, receiveData.length);

        serverSocket.receive(receivePacket);

        System.out.println(new String(receivePacket.getData()));

        InetAddress IPAddress = receivePacket.getAddress();
        int port = receivePacket.getPort();

        while (true) {

            System.out.println("Type some message to display at client end");
            String message = in.nextLine();

            sendData = message.getBytes();

            System.out.println("Message sent from the server:" + new String(sendData));

            DatagramPacket sendPacket =
                    new DatagramPacket(sendData, sendData.length, IPAddress, port);

            serverSocket.send(sendPacket);
        }
    }
}
```

---

### **OUTPUT**

**Server Side**

```
***Server Side***
Connected with Client
Type some message to display at client end
hello how are you
Message sent from the server:hello how are you
Type some message to display at client end
I am studying at SMVITM Bantakal
Message sent from the server:I am studying at SMVITM Bantakal
```

**Client Side**

```
*****Client Display Terminal****
Message typed at server side is : hello how are you
Message typed at server side is : I am studying at SMVITM Bantakal
```
