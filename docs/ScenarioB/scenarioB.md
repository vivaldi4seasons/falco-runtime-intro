# Scenario B: (Memory Dump)


![Memory dumping](../../assets/memoryDumping.webp)



Trin

    sudo dd if=/dev/mem of=mem_dump.bin bs=1M count=100 (dd: failed to open '/dev/mem': No such file or directory)
    sudo dd if=/proc/kcore of=mem_dump.bin bs=1M count=100