# Scenario B: (Memory Dump)



<!-- <img src="../../assets/memoryDumping.webp" align="center" width="50%" height="50%"/> -->

<div style="text-align: center;">
  <img src="../../assets/memoryDumping.webp" width="50%" height="50%" />
</div>

Trin

    sudo dd if=/dev/mem of=mem_dump.bin bs=1M count=100 (dd: failed to open '/dev/mem': No such file or directory)
    sudo dd if=/proc/kcore of=mem_dump.bin bs=1M count=100