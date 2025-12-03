Lets start with the classic.

Here is what I learned about **System Design Interview: Consistent Hashing**.

Working in a start up community: One Eleven has allowed me to talk to startup founders. Spending time with startup founders has shown me how critical scalability really is.
That push me to pick up the _System Design Interview_ book.

However, reading alone is never enough. Once you actually start implement these concepts, you start to see the gaps between yourself and the distributed system. Such as which algorithms to choose and why.

# Consistent Hashing
As data grows, a single server is never enough. To handle scale, we often need dozens or even hundreds of servers to store key-value pairs.
That means we need a way to distribute data across multiple servers efficiently.

So how do we do that? 

The answer is **Hashing**!

Assuming you have 4 servers, traditional hashing (what we learned in school) looks like this:
You compute `key % numberOfServers` to decide which server stores the data.

But what happens when a server gose down?

| With 4 Servers | With 3 Servers |
| :-------: | :------: |
|  16 % 4 = 0  | 14 % 3 = 1 | 
|  19 % 4 = 3 | 19 % 3 = 1  | 
|  91 % 4 = 3  | 91 % 3 = 1 |

Notice how many keys now map to different servers when just one server is removed.
This means massive data reallocation, which is expensive and slow.

To reduce unneccessary reallocation, we need a better approach.

Enter **Consistent Hashing**!

In real world scenarios, adding servers to increase capacity and removing servers due to failures happens all the time. Traditional hashing causes a high amount of movement when this happens.

**Consistent Hashing** resolves this by using a **hash ring**:

1. Imagine all hash valus arranged in a circle.
2. Each server is placed somewhere on that circle.
3. To find where a key belongs, you go clockwise until you reach the next server.

This design brings **stability**: adding or removing a server only affects a small portion of the keys, instead of all of them.

But there's another issue: What if servers cluster in the same part of the ring?
That can create hotspots where one server holds too many keys.

Thats why we introduce **Virtual Nodes**,

Each physical server is mapped to multiple points on the ring. This spreads load more evely and avoids hotspots.

There's much more depth behind these concepts, so if you're interested, please check out _System Design Interview: Consistent Hashing_.


## Description
### Initialization/Core Setup
* number of servers: The number of server in the system.
* number of virtual node: The number of virtual node for each server
* Hash Ring Size = 2 ** 10 = 1024
  
Features to support:
* Adding Key Value Pairs
* Adding Server
* Removing Server


### Data Structure

### Hash Ring structure:
At first, I thought about creating an array of size 1024(hashRing size) and storing servers directly at each index.
But most positions on the ring would be empty, which makes it inefficient.

Instead, I store the hashRing as an array of `{pos: number, serverId: number}`. 
Each entry represents a virtual node placed on the ring.

Seperately, I maintain a server map that stores all key-value pairs: `{serverId: {key: value}}`.

### Algorithm:
We need an efficient algorithm for inserting and locating nodes on the ring.
I initially considered using a heap to maintain a sorted arr of the hashRing.
Note that heaps are optimized for retreiving min/max, but **not** inserting elements at sorted positions.

Thus, the algorithm I ended up choosing is: Binary Search
* It provides `O(log n)` loockup/insert time.
* Thus, it is important to maintain the hashRing sorted.

Hash Algorithm:
* Name an algorithm that does not work and explain why
* name an algorithm that works and explain why

### Edge Cases to Consider
* What happens when the hash of a key is larger than every virtual node's position?
  * Wrap around to the first node in the ring. Update Binary Search.
* What if all virtual nodes cluster together?
  * Virtaul nodes help distribute them evenly.
* What if a server is removed shilw holding data?
  * Rehash each key and reinsert based on the ring. 

### Functions
contructor()
- Parameter: numServers, vnodeCount
- Data Structure:
  - this.hashRing = []
  - this.servers = {}
  - this.numberOfServer = numServers;
  - this.vnodeCount = vnodeCount
- Calls `addServers()` for each server count.

addServers()
* Adds the server to this.servers
* Adds its virtual nodes to the ring
* Sorts the ring for binary search

removeServer()
* Extracts all key-value pairs
* Removes the corresponding virtual nodes
* Re-adds the keys so they redistribute across the ring

addKeyValue(key, value)
* hash the key
* locate the next clockwise server via binary search
* insert into that server's store


### Helper function
* addVirtualNode()
* binarySearch()
* Hash function()
* hash()

```
class ConsistentHasRing {
  constructor(numServers, vnodeCount = 4){
    this.hashRing = [];
    this.servers = {};
    this.numberOfServer = numServers;
    this.vnodeCount = vnodeCount;

    for (let s = 0; s < numServers; s ++) {
      this.addServers(s)
    }
  }

  addServers() {
    this.servers[serverId] = {};
    this.addVirtualNodes(serverId);
    this.hashRing.sort((a, b) => a.pos - b.pos)
  }

  removeServer() {
    const key_to_value = this.hashRing[serverId].copy()
    delete this.hashRing[serverId];
    this.hashRing = this.hashRing.filter( n => n.serverId !== serverId);
    for (key, value) in key_to_value {
      this.addKeyValue(key, value)
    }
  }

  addKeyValue(key, value) {
    const pos = key % 1024
    const idx = this.binarySearch(pos)
    const serverId = this.hashRing[idx].serverId;

    this.servers[serverId][key] = value;
  }

  // ------------------------
  // Helper Functions
  // ------------------------
  
  addVirtualNodes(serverId){
    for (i = 0; i < this.vnodeCount ; i ++){
      const pos = this.hash(`server-${serverId}-vnode-${i}`) % 1024;
      this.insertIntoRing(pos, serverId);
    }
  }

  insertIntoRing(pos, serverId){
    const idx = this.binarySearch(pos)
    this.hashRing.splice(idx, 0, {pos, serverId})
  }

  binarySearch(key){
    let left = 0;
    let right = this.hashRing.length -1;

    while (left <= right) {
      const mid = (left + right) >> 1;
      const midVal = this.hashRing[mid].pos;

      if (midVal < key) {
        left = mid + 1;
      } else {
        right = mid - 1;
      }
    }
    return left === this.hashRing.length ? 0 : left;
  }

  hash(str) {
    let h = 0;
    for (let c of str) h = (h * 31 + c.charCodeAt(0)) % 1024;
    return h
  }
}
```

So what does Consistent hashing taught me about life? Being **consistent** is a **key** **value** in life.

BUG: what happened when the last server has hash 1023 and the key value is 1024, did we take care about the edge case?
PS. there is a lot of missing part at this chapter, it is not intended to be a perfectly working Router. Feel free to make it a fuller working things.






