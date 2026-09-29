

Latency comparison (starter)
============================

Kernel bypass filter baked by KBF Studio (cpp mode).

How to run
----------
make && ./kbf_filter             # simulate
sudo ./kbf_filter --live eth0    # real NIC (Linux)
./kbf_filter --udp 5555          # UDP port

Options: --max-packets N   --seconds S (live)   --out DIR (snapshot files)   --json (machine-readable)

What is inside
--------------
- Axon feed [data_stream]
- Network Card [nic]
- Kernel / CPU Path [kernel_path]
- Kernel Bypass [kernel_bypass]
- Memory Store [memory]
- Snippet Definer [snippet]
- Timestamp [timestamp]
- Processor [processor]
- Latency Meter [latency_meter]

Wiring
------
Axon feed --> Network Card
Network Card --> Kernel / CPU Path
Network Card --> Kernel Bypass
Kernel / CPU Path --> Memory Store
Kernel Bypass --> Snippet Definer
Snippet Definer --> Timestamp
Timestamp --> Processor
Processor --> Latency Meter
Latency Meter --> Memory Store

Reading the numbers
-------------------
latency = t_mem - t_nic: from the moment the card saw the frame to the moment it sat in application memory.
The Memory Store reports this per path (bypass / kernel), the estimated kernel cost, and the savings.
Simulation uses a virtual clock (exact, repeatable). Live runs use CLOCK_REALTIME with kernel receive timestamps.

Where a real bypass plugs in
----------------------------
Replace the live capture functions (kbf_live_open / kbf_live_read in C and C++, live_generate in the Network Card
tile in Python) with DPDK, AF_XDP or ef_vi. The pipeline, the snippet, the timestamps and the report stay the same.

workspace.kbf.json is the design; open it in KBF Studio with Load to keep editing.