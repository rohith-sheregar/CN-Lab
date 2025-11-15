**PROGRAM 3: ETHERNET LAN WITH N NODES & CONGESTION WINDOW PLOT (NS-2)**


---

### **s3.awk**

```awk
BEGIN {
}
{
}
END {
}
{
    if ($6 == "cwnd_") {
        printf("%f\t%f\n", $1, $7);
    }
}
```

---

### **s3.tcl**

```tcl
set ns [new Simulator]

# NAM file
set namfile [open s3.nam w]
$ns namtrace-all $namfile

# Trace file
set tracefile [open s3.tr w]
$ns trace-all $tracefile

# Finish
proc finish {} {
    global ns namfile tracefile
    $ns flush-trace
    close $namfile
    close $tracefile
    exec nam s3.nam &
    exit 0
}

# Nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]
set n6 [$ns node]
set n7 [$ns node]
set n8 [$ns node]

# Colors & shapes
$ns color 1 Blue
$ns color 2 Red
$n7 shape box
$n7 color Blue
$n8 shape hexagon
$n8 color Red

# Links
$ns duplex-link $n1 $n0 2Mb 10ms DropTail
$ns duplex-link $n2 $n0 2Mb 10ms DropTail
$ns duplex-link $n0 $n3 1Mb 20ms DropTail

# LAN
$ns make-lan "$n3 $n4 $n5 $n6 $n7 $n8" 512Kb 40ms LLQueue/DropTail Mac/802_3

# Orientations
$ns duplex-link-op $n1 $n0 orient right-down
$ns duplex-link-op $n2 $n0 orient right-up
$ns duplex-link-op $n0 $n3 orient right

# Queue limit
$ns queue-limit $n0 $n3 20

# TCP Vegas
set tcp1 [new Agent/TCP/Vegas]
$ns attach-agent $n1 $tcp1

set sink1 [new Agent/TCPSink]
$ns attach-agent $n7 $sink1

set ftp1 [new Application/FTP]
$ftp1 attach-agent $tcp1

$ns connect $tcp1 $sink1
$tcp1 set class_ 1
$tcp1 set packetSize_ 55

set tfile1 [open cwnd1.tr w]
$tcp1 attach $tfile1
$tcp1 trace cwnd_

# TCP Reno
set tcp2 [new Agent/TCP/Reno]
$ns attach-agent $n2 $tcp2

set sink2 [new Agent/TCPSink]
$ns attach-agent $n8 $sink2

set ftp2 [new Application/FTP]
$ftp2 attach-agent $tcp2

$ns connect $tcp2 $sink2
$tcp2 set class_ 2
$tcp2 set packetSize_ 55

set tfile2 [open cwnd2.tr w]
$tcp2 attach $tfile2
$tcp2 trace cwnd_

# Schedule
$ns at 0.5 "$ftp1 start"
$ns at 1.0 "$ftp2 start"
$ns at 5.0 "$ftp2 stop"
$ns at 5.0 "$ftp1 stop"
$ns at 5.5 "finish"

$ns run
```

---

### **RUN COMMAND**

```
sudo ns s3.tcl
```

---

### **PROCESS CWND FOR TCP VEGAS**

```
awk -f s3.awk cwnd1.tr > TCPVegas
```

### **GRAPH FOR TCP VEGAS**

```
xgraph -x "Time(sec)" -y "CongestionWindowSize" -t "Congestion Window graph for TCP1" TCPVegas
```

---

### **PROCESS CWND FOR TCP RENO**

```
awk -f s3.awk cwnd2.tr > TCPReno
```

### **GRAPH FOR TCP RENO**

```
xgraph -x "Time(sec)" -y "CongestionWindowSize" -t "Congestion Window graph for TCP2" TCPReno
```




