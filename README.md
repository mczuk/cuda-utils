# cuda-utils
Collection of tools, snippets and resources related to CUDA

## Flush CUDA memory

### Problem description

GPU memory is allocated but `nvidia-smi` does not show any processes using the memory.

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.87.01    Driver Version: 418.87.01    CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1070    Off  | 00000000:01:00.0  On |                  N/A |
| 33%   46C    P8    11W / 151W |   5164MiB /  8116MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  TITAN RTX           Off  | 00000000:21:00.0 Off |                  N/A |
| 41%   34C    P8    14W / 280W |  23176MiB / 24190MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      2432      G   /usr/lib/xorg/Xorg                          4713MiB |
|    0     18397      G   ...uest-channel-token=17664918005394741259    80MiB |
|    0     20182      G   ...quest-channel-token=4371973074826871876    83MiB |
|    0     95331      G   ...PyCharm-P/ch-0/201.7223.92/jbr/bin/java    37MiB |
|    0    124015      G   compiz                                       246MiB |
+-----------------------------------------------------------------------------+
```

### Solution

Run the following command:

```
sudo fuser -v /dev/nvidia*
```

and you will find the following:

```
                     USER        PID ACCESS COMMAND
/dev/nvidia0:        root       2432 F...m Xorg
                     czuk      18397 F...m slack
                     czuk      18435 F...m slack
                     czuk      20182 F...m skypeforlinux
                     czuk      20694 F...m skypeforlinux
                     czuk      95331 F...m java
                     czuk      124015 F...m compiz
/dev/nvidia1:        root       2432 F...m Xorg
                     czuk      18397 F...m slack
                     czuk      18435 F...m slack
                     czuk      20182 F...m skypeforlinux
                     czuk      20694 F...m skypeforlinux
                     czuk      95331 F...m java
                     czuk      121286 F...m python                   <----
                     czuk      121347 F...m python                   <----
                     czuk      121392 F...m python                   <----
                     czuk      121437 F...m python                   <----
                     czuk      121476 F...m python                   <----
                     czuk      124015 F...m compiz
(...)
```

then kill the processes, in this case the python one:

```
kill -9 PID
```


