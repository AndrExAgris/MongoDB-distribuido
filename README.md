# MongoDB-distribuido

---

### **1. Initialize Each Shard**

#### **Shard 1**
1. Connect to `mongo-shard1`:
   ```bash
   docker exec -it mongo-shard1 mongosh --host mongo-shard1:27021
   ```

2. Initiate the replica set:
   ```javascript
   rs.initiate({
     _id: "shard1ReplSet",
     members: [{ _id: 0, host: "mongo-shard1:27021" }]
   });
   ```

---

#### **Shard 2**
1. Connect to `mongo-shard2`:
   ```bash
   docker exec -it mongo-shard2 mongosh --host mongo-shard2:27022
   ```

2. Initiate the replica set:
   ```javascript
   rs.initiate({
     _id: "shard2ReplSet",
     members: [{ _id: 0, host: "mongo-shard2:27022" }]
   });
   ```

---

#### **Shard 3**
1. Connect to `mongo-shard3`:
   ```bash
   docker exec -it mongo-shard3 mongosh --host mongo-shard3:27023
   ```

2. Initiate the replica set:
   ```javascript
   rs.initiate({
     _id: "shard3ReplSet",
     members: [{ _id: 0, host: "mongo-shard3:27023" }]
   });
   ```

---

### **2. Add Shards to the Router**

1. Connect to `mongo-router`:
   ```bash
   docker exec -it mongo-router mongosh --host mongo-router:27017
   ```

2. Add the shards:
   ```javascript
   sh.addShard("shard1ReplSet/mongo-shard1:27021");
   sh.addShard("shard2ReplSet/mongo-shard2:27022");
   sh.addShard("shard3ReplSet/mongo-shard3:27023");
   ```

3. Verify the cluster:
   ```javascript
   sh.status();
   ```

---

### **3. Enable Sharding**

1. Enable sharding for a database:
   ```javascript
   sh.enableSharding("testDB");
   ```

2. Shard a collection:
   ```javascript
   sh.shardCollection("testDB.testCollection", { _id: "hashed" });
   ```

---


