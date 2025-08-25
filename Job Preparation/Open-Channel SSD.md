![[Pasted image 20250822160820.png | 400]]
- Open-Channel SSDs are storage devices that let hosts take full control over data placement and I/O scheduling.
- Require a host-based Flash Translation Layer (FTL)
- How does the host benefit:
	- Fine-grained control over data placement, the host can directly decide **which physical blocks** store which data. This lets applications (like databases or file systems) align their I/O patterns with flash characteristics, reducing write amplification and increasing performance.
	- Some type of caching system for the SSD, where 'hot', 'cold' zones exist. Where 'hot' is (frequently updated) and 'cold' is kept in different flash block. reduces unnecessary copying during garbage collection → **less write amplification → longer flash endurance**.
	- File systems, databases, or distributed storage engines (like RocksDB, Ceph, or MySQL) can be modified to interact directly with flash.