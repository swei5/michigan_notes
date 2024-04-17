[[2024-04-16]] #Database #DBMS 

**Recovery algorithms** are techniques to **ensure database consistency, transaction atomicity, and durability** despite failures. It has two parts:
1. Actions during normal transaction processing to ensure that the DBMS can recover from a failure (Lecture 21)
2. **Actions after a failure to recover the database to a state that ensures atomicity, consistency, and durability** (Today)

### ARIES
**ARIES** stands for Algorithms for Recovery and Isolation Exploiting Semantics. Not all systems implement ARIES exactly as defined in this paper but they're close enough.

The main ideas of ARIES are as followed:
- **Write-Ahead Logging** ([[21 Logging#Write-Ahead Log (WAL)|WAL]])
	- Any change is recorded in log on stable storage before the database change is written to disk
	- Must use **STEAL + NO-FORCE** buffer pool policies
- **Repeating History During Redo**
	- On restart, retrace actions and **restore database to exact state before crash**
- **Logging Changes During Undo**
	- Record undo actions to log to ensure action is not repeated in the event of repeated failures
		- Undo uncommitted changes