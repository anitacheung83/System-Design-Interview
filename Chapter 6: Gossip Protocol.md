# Goosip Protocol

It is important to notice the difference between Goosip and Broadcast. Gossip send the notification to k random peers and not everyone.
Membership list should not be shared to everyone, it should be a deep copy.

Why send all members info instead of itself?
- Because Goosip spreads information transitively.
- If I only send my own heartbeat, information spreads slowly.
- If I send everything I know, information spreads exponentially.

How does node discover new member?
A) Create a function add server
B) When receive and if the member does not exists, I add the new member.

```js

class Member() {
  constructor(id){
    this.id = id
    this.heartbeat = 0
    this.time = 0
  }
}

class Server() {
  constructor(id) {
    this.id = id
    this.peers = {}
    this.member = {
      [id]: new Member(id)
    }
  }

  start(){
    setInterval(() => this.goosip, 1000)
  }
  
  goosip(){
    const me = this.members.id[this.id]
    me.heartbeat ++
    me.lastSeen = Date.now()

    const peers = pickRandom(peers, 2)

    for (const peer of peers){
      peer.receive(this.members)
    }
  }

  receive(remoteMembers){
    for (const member of remoteMembers){
      const remote = remoteMembers[member]
      const local = this.members[member]

      if (!local || remote.heartbeat > local.heartbeat){
        this.members[id] = {...remote}
      }
    }
  }
}
```

```js
servers = []
for (int i = 0; i < 6; i ++){
  let server = new Server(i)
  servers.push(server)
}

members = {}

for (const server of servers){
  let member = new Member(server.id, server.heartbeat, server.time)
  members[server.id] = members
}

for (server of servers){
  server.members = members
  server.peers = members
}

```
Is my code heading to the right direction? 
How do I update server heartbeat periodically? `setInterval`?
When I update heartbeat, do I need to send signal to all the server?
