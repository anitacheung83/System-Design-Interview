
# Goosip Protocol

It is important to notice the difference between Goosip and Broadcast. Gossip send the notification to k random peers and not everyone.
Membership list should not be shared to everyone, it should be a deep copy.

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

for (server of servers){
  let member = new Member(server.id, server.heartbeat, server.time)
  members[server.id] = members
}

for (server of servers){
  server.members = members
}

```
Is my code heading to the right direction? 
How do I update server heartbeat periodically? `setInterval`?
When I update heartbeat, do I need to send signal to all the server?
