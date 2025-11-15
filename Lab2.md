**PROGRAM 2: PING MESSAGE TRANSMISSION IN 6-NODE NETWORK (NS-2)**


---

### **s2.awk**

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
    printf("No of packets dropped: %d\n", count);
}
```

---

### **s2.tcl**

```tcl
# create simulator
set ns [new Simulator]

# nam file
set namfile [open s2.nam w]
$ns namtrace-all $namfile

# trace file
set tracefile [open s2.tr w]
$ns trace-all $tracefile

# finish procedure
proc finish {} {
    global ns namfile tracefile
    $ns flush-trace
    close $namfile
    close $tracefile
    exec nam s2.nam &
    exec awk -f s2.awk s2.tr &
    exit 0
}

# create nodes
set n0 [$ns node]
set n1 [$ns node]
set n2 [$ns node]
set n3 [$ns node]
set n4 [$ns node]
set n5 [$ns node]
set n6 [$ns node]

# links
$ns duplex-link $n1 $n0 1Mb 10ms DropTail
$ns duplex-link $n2 $n0 1Mb 10ms DropTail
$ns duplex-link $n3 $n0 1.75Mb 20ms DropTail
$ns duplex-link $n4 $n0 1Mb 10ms DropTail
$ns duplex-link $n5 $n0 1Mb 10ms DropTail
$ns duplex-link $n6 $n0 1Mb 10ms DropTail

# nam orientations
$ns duplex-link-op $n0 $n1 orient right
$ns duplex-link-op $n0 $n2 orient left
$ns duplex-link-op $n0 $n3 orient right-up
$ns duplex-link-op $n0 $n4 orient right-down
$ns duplex-link-op $n0 $n5 orient left-up
$ns duplex-link-op $n0 $n6 orient left-down

# ping agent recv
Agent/Ping instproc recv {from rtt} {
    $self instvar node_
    puts "node [$node_ id] received ping answer from $from with round-trip-time $rtt ms"
}

# ping agents
set p1 [new Agent/Ping]
set p2 [new Agent/Ping]
set p3 [new Agent/Ping]
set p4 [new Agent/Ping]
set p5 [new Agent/Ping]
set p6 [new Agent/Ping]

# attach
$ns attach-agent $n1 $p1
$ns attach-agent $n2 $p2
$ns attach-agent $n3 $p3
$ns attach-agent $n4 $p4
$ns attach-agent $n5 $p5
$ns attach-agent $n6 $p6

# queue limits
$ns queue-limit $n0 $n4 1
$ns queue-limit $n0 $n5 2
$ns queue-limit $n0 $n6 2

# connections
$ns connect $p1 $p4
$ns connect $p2 $p5
$ns connect $p3 $p6

# send pings
$ns at 0.2 "$p1 send"
$ns at 0.4 "$p2 send"
$ns at 0.6 "$p3 send"
$ns at 1.0 "$p4 send"
$ns at 1.2 "$p5 send"
$ns at 1.4 "$p6 send"

$ns at 2.0 "finish"

$ns run
```

---

### **RUN COMMAND**

```
sudo ns s2.tcl
```


