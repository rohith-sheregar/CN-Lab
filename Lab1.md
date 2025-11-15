**PROGRAM 1: THREE NODE POINT-TO-POINT NETWORK (NS-2)**


---

### **s1.awk**

```awk
BEGIN {
    count = 0;
}
{
    event = $1;
    if (event == "d") {
        count++;
    }
}
END {
    printf("\nNumber of packets dropped is: %d\n", count);
}
```

---

### **s1.tcl**

```tcl
# create new simulation instance
set ns [new Simulator]

# open trace file
set tracefile [open s1.tr w]
$ns trace-all $tracefile

# open nam file
set namfile [open s1.nam w]
$ns namtrace-all $namfile

# define finish procedure
proc finish {} {
    global ns tracefile namfile
    $ns flush-trace
    close $tracefile
    close $namfile
    exec nam s1.nam &
    exec awk -f s1.awk s1.tr &
    exit 0
}

# create 3 nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]

# labels
$n0 label "TCPSource"
$n2 label "TCPSink"

# color
$ns color 1 red

# links
$ns duplex-link $n0 $n1 1Mb 10ms DropTail
$ns duplex-link $n1 $n2 1Mb 10ms DropTail

# queue size between n1 and n2
$ns queue-limit $n1 $n2 5

# TCP agent
set tcp [new Agent/TCP]
$ns attach-agent $n0 $tcp

# TCP sink
set tcpsink [new Agent/TCPSink]
$ns attach-agent $n2 $tcpsink

# FTP
set ftp [new Application/FTP]
$ftp attach-agent $tcp

# connect agents
$ns connect $tcp $tcpsink

# set color
$tcp set class_ 1

# schedule
$ns at 0.2 "$ftp start"
$ns at 2.5 "$ftp stop"
$ns at 3.0 "finish"

$ns run
```

---

### **RUN COMMANDS**

```
sudo ns s1.tcl
```

---

### **s1graph (file data)**

(Create a file named **s1graph** and paste this:)

```
0.25 0
0.50 0
0.75 0
1.00 0
1.25 9
1.50 12
1.75 15
2.00 13
```

---

### **XGRAPH COMMAND**

```
xgraph -x "Bandwidth(Mbps)" -y "No of packets dropped" -t "Performance analysis" s1graph
```

---

