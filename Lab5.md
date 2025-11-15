**PROGRAM 5: SLIDING WINDOW PROTOCOL**

```java
import java.util.Arrays;

public class SlidingWindowProtocol {

    private int windowSize;
    private int[] frames;
    private boolean[] ack;

    public SlidingWindowProtocol(int windowSize, int frameCount) {
        this.windowSize = windowSize;
        this.frames = new int[frameCount];
        this.ack = new boolean[frameCount];

        for (int i = 0; i < frameCount; i++) {
            frames[i] = i;
            ack[i] = false;
        }
    }

    public void sendFrames() {
        int sendIndex = 0;

        while (sendIndex < frames.length) {

            for (int i = 0; i < windowSize && (sendIndex + i) < frames.length; i++) {
                System.out.println("Sending frame: " + frames[sendIndex + i]);
            }

            for (int i = 0; i < windowSize && (sendIndex + i) < frames.length; i++) {
                ack[sendIndex + i] = receiveAck(sendIndex + i);
            }

            while (sendIndex < frames.length && ack[sendIndex]) {
                sendIndex++;
            }
        }
    }

    private boolean receiveAck(int frame) {
        System.out.println("Receiving ack for frame: " + frame);
        return true;
    }

    public static void main(String[] args) {
        int windowSize = 4;
        int frameCount = 10;

        SlidingWindowProtocol swp = new SlidingWindowProtocol(windowSize, frameCount);
        swp.sendFrames();
    }
}
```

---

### **OUTPUT**

```
Sending frame: 0
Sending frame: 1
Sending frame: 2
Sending frame: 3
Receiving ack for frame: 0
Receiving ack for frame: 1
Receiving ack for frame: 2
Receiving ack for frame: 3
Sending frame: 4
Sending frame: 5
Sending frame: 6
Sending frame: 7
Receiving ack for frame: 4
Receiving ack for frame: 5
Receiving ack for frame: 6
Receiving ack for frame: 7
Sending frame: 8
Sending frame: 9
Receiving ack for frame: 8
Receiving ack for frame: 9
```
