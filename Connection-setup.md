# Connection setup
---
1. Install HackRF dependencies:

```bash
sudo apt install cmake libusb-1.0-0-dev pkg-config libfftw3-dev
```

2. Download and compile HackRF source code: 

```bash
git clone https://github.com/mossmann/hackrf
cd hackrf
cmake . 
make
sudo make install
```

3. Install GNU Radio:

```bash
sudo apt install gnuradio gr-osmosdr
```

4. Connect HackRF and ensure it is detected:

```bash  
hackrf_info
```

5. Open GNU Radio Companion and create a flowgraph:

- Add HackRF source block 
- Set sample rate to 2 MS/s
- Add complex to mag block and QT GUI waterfall sink

6. Tune to 908 MHz Z-Wave frequency 

7. Capture traffic and save data to file:

```python
self.blocks_file_sink_0 = blocks.file_sink(gr.sizeof_gr_complex*1, "/tmp/hackrf_capture", False) 
self.blocks_file_sink_0.set_unbuffered(False)
```

8. You can also capture using Python scripting.

So in summary, install HackRF tools, connect SDR, create a GNU Radio flowgraph with HackRF source, filters, sinks, and capture traffic to file for analysis. Make sure to tune to the correct frequency.
