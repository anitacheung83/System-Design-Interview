
# Goosip Protocol
```js

class Membership() {
  constructor(id){
    this.id = id
    this.heartbeat = 0
    this.time = 0
  }

  setHeartbeat(heartbeat, time) {
    this.heartbeat = heartbeat
    this.time = time
  }

}

class Server() {
  constructor(id) {
    this.id = id
    this.membership_lists = []
    this.heartbeat = 0
    this.time = time.now()
  }

  updateHeartbeat(serverId){
    this.membership_list[id].heartbeat = heartbeat
    this.membership_list[id].time = time
  }

  sendHeartbeat(){
    const heartbeat = this.heartbeat + time.now() - this.time()
    for (server of servers){
      server.updateHeartbeat(heartbeat, this.time())
    }

    this.heartbeat = heartbeat;
    this.time = time.now()
  }

  addServers(){
  }

  removeServers(){
  }
}
```

```js
servers = []
for (int i = 0; i < 6; i ++){
  let server = new Server(i)
  servers.push(server)
}



```
