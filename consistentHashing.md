Lets start with the classic:

Here is what I learned about System Design Interview: Consistent Hashing.

Wanting to work as a developer at a Start Up has made me realized how important scaling is. Therefore, I decided to start learning about System Design.
However, reading about it is never enough, when you truly start implementing it, you will learn a lot more and realized the gaps between you and the distributed system.
Such as which algorithm to choose and why.

# Consistent Hashing
In the world where there are more and more data, having 1 server is not enough to support it. Therefore, it is crucial to have more than 1, maybe even a thousands of servers to store key-value pairs. 
Therefore you need to distribute the data.

How do you do so?
The answer is Hashing!

Assuming you have 4 servers:
With the traditional hashing that was taught in school:
You will hash the key % 4 = to see which server it belongs.

Now, what happend with a server goes down.

For example,
Before
14 % 4
19 % 4
61 % 4

After
14 % 3
19 % 3
61 % 3

However, adding a servers to increase capacity and removing a server due to system failure is unavoidable in the real world.

With Traditional hashing, a lot of data will ended up in a different server eventhough there is no need for them to move.

Here comes the hashRing.
1. A Ring
2. Go clockwise to find the next avaliable servers.
3. 
Explain why use a hash Ring, because of consistency and adding a server and removing a server does not need to reorganize the key value store.

Now what happend with all the servers if the servers is crowded together, then the problem where most of the key value store happened in a server might
Virtual Ring, why is it needed?

Which hash Algorithm to use?


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
    delete this.hashRing[serverId];
    this.hashRing = this.hashRing.filter( n => n.serverId !== serverId);
  }

  addKeyValue() {

  }

  // ------------------------
  // Helper Functions
  // ------------------------

  binarySearch(key){
    
  }
}
```

BUG: what happened when the last server has hash 1023 and the key value is 1024, did we take care about the edge case?
PS. there is a lot of missing part at this chapter, it is not intended to be a perfectly working Router. Feel free to make it a fuller working things.






