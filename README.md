Download Link: https://assignmentchef.com/product/solved-mips-simulator-data-cache-project-5-cs-3339
<br>
<h1>PROBLEM STATEMENT</h1>




In this project, you will further enhance your simulator to model pipeline stalls due to memory latency, and you will simulate a data cache to study the performance impact of caching.  You should begin with a copy of your Project 3 submission.  Additionally, I have provided a class skeleton for a CacheStats class intended to be instantiated inside your existing CPU class.  You may add any functions, function parameters, etc. to it that you want.




The simulated data cache should store 1 KiB (1024 Bytes) of data in block sizes of 8 words (32 bytes).  It should be 4-way set associative, with a round-robin replacement policy and a write policy of write-back write-allocate.  All blocks in the cache are initially invalid.  For simplicity, assume the cache has no write buffer: a store must be completely finished before the processor can proceed.  Since this is a data cache, only loads and stores access it; instruction fetches should still be assumed to hit in a perfect I-cache with immediate access (i.e., there is never a stall for an instruction fetch).




Note that since you only have to model the hit/miss/timing behavior of the cache, you do not have to actually store any <em>data</em> in your cache model.  It is sufficient to simulate the valid, tag, and dirty bits as well as the round-robin replacement policy.




Your cache model will calculate and report the following statistics:

<ul>

 <li>The total number of <strong>accesses</strong>, plus the number that were <strong>loads</strong> <strong>stores</strong></li>

 <li>The total number of <strong>misses</strong>, plus the number caused by <strong>loads</strong> <strong>stores</strong></li>

 <li>The number of <strong>writebacks</strong></li>

 <li>The <strong>hit ratio </strong></li>

</ul>




Every time an access is made to the cache model, the cache model should return the number of cycles that the processor must stall in order for that access to complete.  It takes 0 cycles to do a lookup or to hit in the cache (i.e., data that is hit will be returned or written immediately).  A read access to the next level of the memory hierarchy (e.g., main memory) has a latency of 30 cycles, and a write access has a latency of 10 cycles.  Note that an access resulting in the replacement of a dirty line requires both a main memory write (to write back the dirty block) and a read (to fetch the new block) consecutively.  Because the cache has no write buffer, all stores must stall until the write is complete.




<strong><em>Before computing the cache statistics, be sure to drain all the dirty data from the cache, i.e. write it back.</em></strong>  Count these as writebacks, but do not count any stalls/latency resulting from them.




Inside your Stats class, add a new function similar to the bubble() and flush() functions.  This stall() function should stall the entire pipeline for a specified number of cycles.  The Stats class should track the total number of stall cycles that occur during program execution.




Your simulator will report the following statistics at the end of the program:

<ul>

 <li>The exact <strong>number of clock cycles</strong> it would take to execute the program on this CPU</li>

 <li>The <strong>CPI</strong> (cycle count / instruction count)</li>

 <li><em>The <strong>number of bubble cycles</strong> injected due to data dependencies (unchanged from Project 3) </em></li>

 <li><em>The <strong>number of flush cycles</strong> in the shadows of jumps and taken branches (also unchanged from Project 3 – not the values from Project 4) </em></li>

 <li>The <strong>number of stall cycles</strong> due to cache/memory latency, new for Project 5 The <strong>data cache statistics</strong> reported by the cache model</li>

</ul>




I have provided the following new files:

<ul>

 <li>A h class specification file, to which you’ll need to add member variables</li>

 <li>A cpp class implementation file, which you should enhance with code to model the described cache and count accesses, misses, and writebacks</li>

 <li>A new Makefile</li>

</ul>




In addition to enhancing the CacheStats.h/.cpp skeleton, you will need to modify your existing Stats.h/Stats.cpp in order to implement memory stalls.  You will also need to modify CPU.h to instantiate a CacheStats object, and CPU.cpp to call both CacheStats and Stats class functions appropriately to model cache behavior and resulting pipeline stalls.  You’ll also need to change CPU::printFinalStats() to match my expected output format (see below).




<strong> </strong>

<h1>ASSIGNMENT SPECIFICS</h1>




Here are the steps you should follow for this project:




<ul>

 <li>Begin by copying all of your Project 3 files into a new Project 5 directory</li>

 <li>Untar and add the additional files from TRACS to your project5 directory</li>

 <li>Write your name in the header of CacheStats.cpp and CacheStats.h</li>

 <li>Add the #include for CacheStats.h into your CPU.h file</li>

 <li>Instantiate a CacheStats object named cache in your CPU.h similar to your stats object 6) Complete the functions for stats.stall, cache.access, and others as needed.</li>

 <li>Modify CPU.cpp to call cache.access HINT:  you probably want to do this in CPU::mem()</li>

 <li>Also modify CPU.cpp to call cache.printFinalStats() and remove any unnecessary output</li>

 <li>Check your results using submit_test script</li>

 <li>Upload to TRACS before the deadline – verify by getting the email confirmation</li>

</ul>




You can compile and run the simulator program identically to previous projects, and test it using the same *.mips inputs.  Only sssp.mips yields interesting cache behavior; I’ll only grade your code using this one.




If you examine CacheStats.h, you’ll notice that I’ve already defined constants for you for all of the cache configuration options you’ll need (e.g., number of sets, number of ways, block size, read miss latency, etc.).  You should not need to change any of these defines.  They are defined to be modifiable from the compilation command line, but for this project, I will not change any of them.  You can even get away with not using these defined constants if you prefer.







The following is the expected result for sssp.mips.  Your output must match this format verbatim.  Compare your output to the provided sssp.out file in the tarball using the diff command as in prior projects:

<strong> </strong>

CS 3339 MIPS Simulator

Cache Config: 1024 B (32 bytes/block, 8 sets, 4 ways)

Latencies: Lookup = 0 cycles, Read = 30 cycles, Write = 10 cycles Running: sssp.mips




7 1




Program finished at pc = 0x400440  (449513 instructions executed)




Cycles: 2040814

CPI: 4.54




Bubbles: 1125724

Flushes: 51990

Stalls:  413580




Accesses: 197484

Loads: 146709

Stores: 50775

Misses: 12044

Load misses: 8559

Store misses: 3485

Writebacks: 5229

Hit Ratio: 93.9%




The CacheStats skeleton already includes code to disable the cache (i.e., all loads result in a read access to the next level of the memory hierarchy, and all stores result in a write access).  To explore the performance impact of adding a cache, you can compile your simulator with the cache disabled:




$ make clean; make CACHE_EN=0




Re-run the simulator on sssp.mips.  What happens to the CPI?  How big is the difference?




Additional Requirements:

<ul>

 <li><strong>Your code must compile with the given </strong><strong>Makefile and run on zeus.cs.txstate.edu </strong></li>

 <li>Your code must be well-commented, sufficient to prove you understand its operation</li>

 <li>Make sure your code doesn’t produce unwanted output such as debugging messages. (You can accomplish this by using the D(x) macro defined in h)</li>

 <li>Make sure your code’s runtime is not excessive</li>

 <li>Make sure your code is correctly indented and uses a consistent coding style</li>

 <li>Clean up your code before submitting: i.e., make sure there are no unused variables, unreachable code, etc.</li>

</ul>


