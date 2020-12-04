## Subscriptores

#### Todos
```sql
SELECT DISTINCT SubscriberKey 
FROM _Subscribers
```

#### Segun Status
```sql
SELECT DISTINCT SubscriberKey 
FROM _Subscribers 
WHERE Status = 'active'
```
*Disponibles: active, bounced, held, unsubscribed*

#### Subscribers que estan en _Sent
```sql
SELECT DISTINCT s.SubscriberKey 
FROM _Sent s 
WHERE s.SubscriberKey IN (SELECT SubscriberKey FROM _Sent) 
```
*Se puede usar otra DataView o DataExtension, como _Open o _Click*

#### Subscribers que no estan en _Open
```sql
SELECT DISTINCT s.SubscriberKey 
FROM _Sent s 
WHERE s.SubscriberKey NOT IN (SELECT SubscriberKey FROM _Open) 
```
*Se puede usar otra DataView o DataExtension, como _Sent o _Click*

## Bounces

#### Todos
```sql
SELECT DISTINCT SubscriberKey 
FROM _Bounce
```

#### HardBounces
```sql
SELECT DISTINCT SubscriberKey 
FROM _Bounce 
WHERE b.BounceCategory = 'Hard bounce'
```

## Metricas

#### Total de Envios (Sents)
```sql
SELECT COUNT(EventDate)
FROM _Sent
```

#### Total de Aperturas (Opens)
```sql
SELECT COUNT(EventDate)
FROM _Open
```

#### Total de Aperturas Unicas (Unique Opens)
```sql
SELECT COUNT(EventDate)
FROM _Open
WHERE IsUnique = 1
```

#### Total de Clicks
```sql
SELECT COUNT(EventDate)
FROM _Click
```

#### Total de Clicks Unicos (Unique Clicks)
```sql
SELECT COUNT(EventDate)
FROM _Click
WHERE IsUnique = 1
```
