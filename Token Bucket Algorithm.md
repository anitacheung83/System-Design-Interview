# System Design Interview: Rate Limiter: Token Bucket Algorithm

setInterval based

Initially I thought about using `setInterval`, however it is not precise (event loop delays), hard to scale if you have many buckets and decoupled from actual request timing.
Each instance creates its own interval -> memory & performance issues at scale.
Hard coded timing.

```js
class TokenBucket {
  constructor(capacity, refillRate){
    this.capacity = capacity;
    this.tokens = capacity;
    this.refillRate = refillRate;
    this.lastRefillTime = Date.now(); // Timestamp of the last refill
  }

  refill() {
    const now = Date.now();
    const elapsed = (now - this.lastRefillTime) / 1000;
    const newToken = elapsed * this.refillRate;

    this.tokens = Math.min(this.capacity, this.tokens + newTokens);
    this.lastRefillTime = now;
  }

  allowRequest() {
    this.refill();
    if (this.token >= 1){
      this.token --;
      return True
    }
    return False
  }
}
```

```js

class TokenBucketAlgorithm{
  /*
  @param limits: the limit per minuite
  */
  constructor(limit: int){
    this.limit = limit
    this.bucket = limit
    refiller()
  }

  receiveRequest(){
    if (this.bucket == 0){
      return "429 Too Many Request"
    }
    this.bucket --
  }

  refiller() {
    setInterval(() => {
      if (this.bucket < limit){
        this.bucket ++
      }
    }, 25000)
  }

}

```
