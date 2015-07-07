---
layout: post
title:  Programmable computing frontiers
date:   2014-07-05 14:00:00
---

Last month saw some *really interesting* announcements from Microsoft and Intel about the combination of field-programmable gate arrays (FPGAs) and traditional stored-program microprocessors (in this case Intel Xeon server CPUs). Microsoft [wedded FPGA chips to its traditional Xeon servers](http://arstechnica.com/information-technology/2014/06/bing-doubles-performance-with-fpgas-will-use-them-in-2015/) through a PCIe card to get a "95 percent improvement in search throughput" with only a "ten percent increase in total system power". The next day Intel [announced plans](http://www.theregister.co.uk/2014/06/18/intel_fpga_custom_chip/) to start selling Xeon server chips with an embedded FPGA slapped on top.

This is an exciting step in what I believe to be a new generation of computation. As we start pushing up against the limits of nanotechnology in our processor fabrication we will need to be more ingenuitive in architecture of our processors, and FPGAs are a baby step in this direction.

The traditional microprocessor has for decades been based upon the fetch-decode-execute model of operation: fetch instruction, decode instruction, execute instruction. We have made some advances in speeding this up ([the instruction pipeline](http://en.wikipedia.org/wiki/Instruction_pipeline) and the instruction cache for example), however this cycle will continue to be a barrier to performance.

FPGAs offer an alternative to this: rather than storing our programs in memory and performing the fetch-decode-execute cycle to run our programs, we actually "re-code" the gates of an FPGA to make it perform a program. There's no fetching and decoding; there's just execution. Reprogrammability goes out the window, however in return massive performance gains can be seen. This is why FPGAs are so popular in media encoding/decoding and cryptocurrency mining: the programs don't change so there's no need for that fetch-and-decode process before execution.

As FPGA technology advances I hope we'll see a greater degree of reprogrammability (right now reprogramming most FPGAs is a complicated process), and perhaps someday soon our processors will be a hybrid of memory-programmable (traditional) and circuit-programmable (FPGA-style) processing. What will our compilers (both static, ahead-of-time, and just-in-time) look like when we have this additional execution target?
