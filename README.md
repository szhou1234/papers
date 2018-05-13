# papers i love
These are papers I love.

Life beyond Distributed Transactions: an Apostate’s Opinion:
This is a bit old (2007) and actually lots of NoSQL databases use the ideas described in this paper. However, I don't agree that message passing should happen on entity level. Take Cassandra for example, each entity is a partition. Can you do message passing on partition level? That would be too many senders/receivers!

Viewstamped replication
The approach is based on primary copy technique. The primary is reponsible for the processing of transactions that uses its objects; it notifies the backups of what it has done. When a replica crashes or is separated from the others by a partition, or when a replica recovers from a crash or a partition is repaired, the replicas are reorganized and a new primary is selected if necessary. We refer to this reorganization as a view change. Once the view change is complete, the (new) primary can continue with transaction processing.
If a view change happens, the effects of a call may or may not survive into the new view. If they do survive, the transaction can commit; otherwise, it must abort. We use a special kind of timestamp called a viewsramp to distinguish the two situations.

https://brooker.co.za/blog/2014/05/19/vr.html
https://www.cs.cornell.edu/fbs/publications/viveLaDifference.pdf (I don't quite understand this)

The idea in VR is hard to follow. VRR is much easier to digest. From https://blog.acolyer.org/2015/03/06/viewstamped-replication-revisited/:
The lifetime of the system is divided into epochs, and epochs are divided into views. When not changing between views or epochs, the system operates normally . Epoch changes are driven by an administrative action to add or remove group members (this may be automated of course). View changes within an epoch are driven by process or communication failure (assumed to be temporary) amongst the members of the epoch.

The protocol description assumes all participating replicas are in the same view. Every message sent from one replica to another contains the sender’s current viewnumber. Replicas only process normal protocol messages containing a view-number that matches the view-number they know. If the sender is behind, the receiver drops the message. If the sender is ahead, the replica performs a state transfer: it requests information it is missing from the other replicas and uses this information to bring itself up to date before processing the message.

there is no leader election as such to chose a new primary. Within an epoch the group members are fixed (even though some may be unreachable or failed). Each of these is assumed to have a unique IP address, and leadership rotates around the group in round-robin order of ascending IP addresses. So when a primary fails, each group member already knows which member is expected to be the next primary.

Paxos Made Simple
https://blog.acolyer.org/2015/03/04/paxos-made-simple/
https://blog.acolyer.org/2015/03/05/paxos-made-live/
proof of Paxos:
http://wwwusers.di.uniroma1.it/~mei/SistemiDistribuiti/Schedule_files/fastpaxosfordummies.pdf
Paxos Made Moderately Complex
http://www.cs.cornell.edu/courses/cs7412/2011sp/paxos.pdf

Zab:
https://blog.acolyer.org/2015/03/09/zab-high-performance-broadcast-for-primary-backup-systems/


How to read papers:
http://muratbuffalo.blogspot.com/2013/07/how-i-read-research-paper.html or http://blizzard.cs.uwaterloo.ca/keshav/home/Papers/data/07/paper-reading.pdf
http://ccr.sigcomm.org/online/files/p83-keshavA.pdf
https://www.eecs.harvard.edu/~michaelm/postscripts/ReadPaper.pdf
