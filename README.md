# SGI Scalable Graphics Compositor Firmware Patcher Task

### Preamble

This was set as a vibe-coding challenge at [Numfum GmbH](//numfum.com) to see how quickly coding models could generate a working solution. It's an equally valid task for real-life humans the ol' fashioned way.

A rare piece of [computer hardware](//irix7.com/techpubs/007-4602-002.pdf) has been found with a newer firmware than is publicly documented, so for those of us interested in preserving vintage computers it's important to extract this. Invasive methods exist (e.g. a [Bus Pirate](//buspirate.com) sniffing JTAG pins, followed by some magic) but this is one of the few known working examples so anything that risks damage should be avoided. Having poked around the connected computer's [OS](//en.wikipedia.org/wiki/IRIX) it was discovered one of the conformance test tools (`sting`) could be made to output an extended log file (normally it doesn't). We'll ignore the knowledge required that such a tool exists (it's undocumented) or that further poking around was required, and start everyone with the same data.

### Starting Point

1. The last known firmware version images are distributed with the OS, `ccx.mcs` and `ctx1.mcs`
2. A log file (`putty.log`) from the `sting` tool containing read _errors_ between these firmware images and what it sees on the connected hardware

The log file contains thousands of entries where the firmware differs:
```
**** ERROR 024000       Miscompare of data at Flash address 0x188294: exp
**** ERROR 024000       0x180060 got 0x580060
```
As alluded to at the start, no AI was going to ever suggest looking at an undocumented conformance testing tool, this was a lucky break by poking around. As it turns out, the more time spent working on a problem the luckier one gets...

### Required Result

A tool to create updated firmware files from the originals and error logs. Such a tool should:

1. Be readable, since it will be shared with other compositor owners
2. Apply *all* the differences from the logs
3. Create error-free firmware images, since if the hardware were re-flashed with these images and errors exist, it may not be recoverable
4. Work with other `sting` tool error logs and source firmware versions, not simply assume these three files exist in a vacuum

### The Obsolete Human

By hand this took around two days to write a tool that does the conversion plus a few more firmware related tasks, with plenty of interruptions from the real world. The `sting` tool was used to verify that the hardware's firmware matches the combined files, with zero _miscompare_ errors.

This hand-rolled solution was written in Python, but any language could be used.
