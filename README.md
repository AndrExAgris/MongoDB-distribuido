# MongoDB-distribuido

---

### **1. Inicializar cada shard**

#### **Shard 1**
1. Entrar em `mongo-shard1`:
   ```bash
   docker exec -it mongo-shard1 mongosh --host mongo-shard1:27021
   ```

2. Inicializar o set de replicação:
   ```javascript
   rs.initiate({
     _id: "shard1ReplSet",
     members: [{ _id: 0, host: "mongo-shard1:27021" }]
   });
   ```

---

#### **Shard 2**
1. Entrar em `mongo-shard2`:
   ```bash
   docker exec -it mongo-shard2 mongosh --host mongo-shard2:27022
   ```

2. Inicializar o set de replicação:
   ```javascript
   rs.initiate({
     _id: "shard2ReplSet",
     members: [{ _id: 0, host: "mongo-shard2:27022" }]
   });
   ```

---

#### **Shard 3**
1. Entrar em `mongo-shard3`:
   ```bash
   docker exec -it mongo-shard3 mongosh --host mongo-shard3:27023
   ```

2. Inicializar o set de replicação:
   ```javascript
   rs.initiate({
     _id: "shard3ReplSet",
     members: [{ _id: 0, host: "mongo-shard3:27023" }]
   });
   ```

---

### **2. Adicionar os shards no router**

1. Entrar em `mongo-router`:
   ```bash
   docker exec -it mongo-router mongosh --host mongo-router:27017
   ```

2. Adicionar os shards:
   ```javascript
   sh.addShard("shard1ReplSet/mongo-shard1:27021");
   sh.addShard("shard2ReplSet/mongo-shard2:27022");
   sh.addShard("shard3ReplSet/mongo-shard3:27023");
   ```

---

### **3. Permitir o sharding no bando de dados**

1. Permitir o sharding no bando de dados:
   ```javascript
   sh.enableSharding("testDB");
   ```

2. Utilizar o shard:
   ```javascript
   sh.shardCollection("testDB.testCollection", { _id: "hashed" });
   ```

---


