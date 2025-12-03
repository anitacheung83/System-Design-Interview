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
* Initialization:
* number of servers
* number of virtual node
* Hash Ring Size = 2 ** 10 = 1024
  
Feature to support:
* Adding Key Value Pairs
* Adding Server
* Removing Server


### Data Structure

### Hash Ring structure:
My initial thoughts on the hashRing structure was to build an array with a size of 1024. However, it will be really slow because not all the idx with store something. 
Therefore, storing a hashRing with an Array of Key Value of Position and Server number. {pos: number, serverId: number}. Where the hashRing will store the virtual servers.
Then, we will have a map of servers storing the server number and the associated key and value store. {serversId: {key: value}}.

### Servers Structure


### Algorithm:
Now that the Hash Ring Structure is determined, its time to pick the most time effiecient algorithm.
The algorithm that I have in mind was Heap and Binary Search.
Heap was elimated because heap is better for sorting, while our goal is to insert the key value pair into a particular spot rather then finding the minimum value and the maximum value.

Binary Search:
* binary search algorithm is a perfect algorithm, there is a lot of insert function and library that uses binary search: such as the famous bisect library.
* Where the runtime is going to be O(log(n))
* It is important to maintain the hash Ring sorted

Hash Algorithm:
* Name an algorithm that does not work and explain why
* name an algorithm that works and explain why

### Edge Cases

### Functions
contructor()
- Parameter: numServers, vnodeCount
- Data Structure:
  - this.hashRing = []
  - this.servers = {}
  - this.numberOfServer = numServers;
  - this.vnodeCount = vnodeCount
- function: initialize add servers

addServers()
- should add virtaul server using a hash Algorthm (insert link)
- sort the hashRing so you can apply binary Search

removeServer()
- get the list of key value pairs
- delete this.servers[serverId];
- remove virtual hashRing from the server
- now for each key value pairs call add key value

addKeyValue(key, value)
- get the hashed value
- binarySearch the hash ring for insertion
- insert the key value pair to the server


### Helper function
addVirtualNode()
binarySearch()
Hash function()

Please see the code below

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

      if (midVal < val) {
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






