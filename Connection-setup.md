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

---
## GNU RADIO

**1. Launch GNU Radio Companion:**

- Open GNU Radio Companion from your application menu or terminal.

**2. Create the Flowgraph:**

- Drag and drop the following blocks onto the canvas:
    - **HackRF Source:** Located in the "Sources" category.
    - **Complex to Mag:** Located in the "Converters" category.
    - **QT GUI Waterfall Sink:** Located in the "Visualizers" category.
    - **File Sink:** Located in the "Sinks" category.

**3. Connect the Blocks:**

- Connect the blocks in this order:
    - HackRF Source -> Complex to Mag -> QT GUI Waterfall Sink -> File Sink.

**4. Configure the Blocks:**

**HackRF Source:**
- Double-click the block to open its properties.
- Set the "Sample Rate" to 2 MS/s.
- Set the "Frequency" to 908 MHz.

**Complex to Mag:**
- No additional configuration needed.

**QT GUI Waterfall Sink:**
- No additional configuration needed.

**File Sink:**
- Double-click the block to open its properties.
- Set the "Filename" to "/tmp/hackrf_capture".
- Set "Unbuffered" to False.

**5. Generate Python Code:**

- Click the "Generate" button in the toolbar to create the Python code for the flowgraph.

**6. Run the Flowgraph:**

- Click the "Execute" button in the toolbar to run the flowgraph.

**7. Observe the Waterfall Sink:**

- You should see the waterfall display in the QT GUI Waterfall Sink.

**8. Capture Traffic and Save Data:**

- The captured data will be saved to the specified file "/tmp/hackrf_capture".

