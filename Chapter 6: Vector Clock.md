# System Design Interview: Design Key-Value Store: Vector Clock

Easy Implementation of changing the number. Where Key is a number and value is also a number

The relationship between Server Class and Vector Clock Class

Server Class
* read(), write()
* store key value pairs

Vector Clock
* 

```js
class Server(){

  constructor(id: int){
    this.id = id;
    this.store = new Map(); 
  }

  read(key){
    return this.store.get(key) || [];
  }

  write(key, value, contextClock = new VectorClock()){
    contextClock.increment(this.id);

    const newVersion = new VersionedValue(value, contextClock.copy());

    const versions = this.store.get(key) || [];
    const survivors = [];

    for (const v of versions) {
      const relation = compare(v.clock, newVersion.clock);

      if (relation === "ANCESTOR){
        // overwritten
        continue;
      }
      survivors.push(v)
    }

    survivors.push(newVersion);
    this.store.set(key, survivors);
  }

}

class VectorClock(){
  constructor(clock = {}) {
    this.clock = {...clock}
  }

  increment(serverId){
    this.clock[serverId] = (this.clock[serverId] || 0) + 1;
  }

  merge(other){
    for (const id in other.clock){
      this.clock[id] = Math.max(this.clock[id] || 0, other.clock[id]);
    }
  }

  copy() {
    return new VectorClock(this.clock);
  }
}

```

```js
function compare(vc1, vc2){
  
}
```

```js
const Sx = new Server("Sx");
const Sy = new Server("Sy");
const Sz = new Server("Sz");

Sx.write("D", "D1");

let ctx = Sx.read("D")[0].clock.copy();
Sx.write("D", "D2", ctx);

ctx = Sx.read("D")[0].clock.copy();
Sy.write("D", "D3", ctx);

// Concurrent
ctx= SX.read("D")[0].clock.copy();
Sz.write("D", "D4", ctx); 

const merged = new VectorClock();
merged.merge(Sy.read("D")[0].clock);
merged.merge(Sz.read("D")[0].clock);

Sx.write("D", "D5", merged)
```
