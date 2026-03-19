One of the biggest challenges in this project was balancing correctness, reliability, and
performance across a bunch of different network conditions. Getting a basic sliding window
protocol working in ideal conditions wasn’t too bad, but things got a lot more complicated once
we had to deal with packet loss, duplication, corruption, and jitter.
Packet corruption (mangling) was especially frustrating. Sometimes packets would arrive
missing fields like seq, which broke assumptions in our code and caused crashes. To fix this, we
had to make the receiver much more defensive by validating every incoming packet: checking
that all required fields existed, were the right type, and matched their checksums before using
them.
Another challenge was figuring out retransmissions. Early on, our sender either retransmitted
too aggressively, which created a lot of unnecessary overhead, or not aggressively enough,
which caused the protocol to stall under jitter or packet loss. Finding a balance between those
two extremes took a lot of trial and error.
Handling out-of-order packets on the receiver side was also tricky. We needed to buffer packets
and only print data once everything before it had arrived, while still sending correct cumulative
ACKs back to the sender. At the same time, we had to make sure duplicate packets didn’t mess
up the logic, which required careful handling.
The hardest part overall was tuning everything to work well under both low-bandwidth and
high-jitter conditions. Small tweaks to things like window size, timeout values, or ACK handling
could fix one test but break another, so debugging became very iterative. We had to constantly
re-run tests and adjust our approach.
