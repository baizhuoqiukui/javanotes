# Lettuce连接池



获取连接

```java
private final Map<StatefulConnection<?, ?>, GenericObjectPool<StatefulConnection<?, ?>>> poolRef = new ConcurrentHashMap<>(32);

private final Map<Class<?>, GenericObjectPool<StatefulConnection<?, ?>>> pools = new ConcurrentHashMap<>(32);

public <T extends StatefulConnection<?, ?>> T getConnection(Class<T> connectionType) {

   // computeIfAbsent key不存在时会从新计算
   GenericObjectPool<StatefulConnection<?, ?>> pool = pools.computeIfAbsent(connectionType, poolType -> {
      return ConnectionPoolSupport.createGenericObjectPool(() -> connectionProvider.getConnection(connectionType),
            poolConfig, false);
   });

   try {

      StatefulConnection<?, ?> connection = pool.borrowObject();

      poolRef.put(connection, pool);

      return connectionType.cast(connection);
   } catch (Exception e) {
      throw new PoolException("Could not get a resource from the pool", e);
   }
}
```

pools主要是用于根据连接类型获取对应连接池

poolRef主要是用于连接的归还场景

- StatefulConnection有几个类型，分别是StatefulRedisConnection(`StatefulRedisPubSubConnection、StatefulRedisMasterSlaveConnection`)、StatefulRedisSentinelConnection、StatefulRedisClusterConnection