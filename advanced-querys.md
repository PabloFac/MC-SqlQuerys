## Advanced Querys

# Query 1: Envio en N dias, sin apertura en N dias
Subscriptores que 
- se les realizo un envío en los ultimos 90 dias, y
- no abrieron ningun email en los ultimos 90 dias
```sql
SELECT Subscriberkey
FROM _Subscribers
WHERE
  Subscriberkey in (Select SubscriberKey from _Sent where EventDate > DATEADD(d,-90,GETDATE())) AND
  Subscriberkey not in (Select Subscriberkey from _Open where EventDate > DATEADD(d,-90,GETDATE()))
```

# Query 2: Sin envio en N dias, sin apertura en N dias
Subscriptores que 
- no se les realizo ningun envio en los ultimos 30 dias, y
- no abrieron ningun email en los ultimos 90 dias
```sql
SELECT Subscriberkey
FROM _Subscribers
WHERE
  Subscriberkey not in (Select SubscriberKey from _Sent where EventDate > DATEADD(d,-30,GETDATE())) AND
  Subscriberkey not in (Select Subscriberkey from _Open where EventDate > DATEADD(d,-90,GETDATE()))
```

# Query 3: No nuevo subscriptor en N dias, sin envio en N dias, sin apertura en N dias
Subscriptores que 
- se registraron hace mas de 30 dias,
- no se les realizo ningun envio en los ultimos 30 dias, y
- no abrieron ningun email en los ultimos 90 dias
```sql
SELECT Subscriberkey
FROM _Subscribers
WHERE
  DateJoined < DATEADD(d,-30,GETDATE()) AND
  Subscriberkey not in (Select SubscriberKey from _Sent where EventDate > DATEADD(d,-30,GETDATE())) AND
  Subscriberkey not in (Select Subscriberkey from _Open where EventDate > DATEADD(d,-90,GETDATE()))
```

# Query 4: Cantidad de Envios a cada subscriptor
```sql
SELECT su.SubscriberKey, COUNT(s.EventDate) AS Envios
FROM _Subscribers su
LEFT JOIN _Sent s ON s.SubscriberKey = su.SubscriberKey
GROUP BY su.SubscriberKey
```

# Query 5: Fecha de ultima apertura de cada subscriptor
```sql
SELECT 
    su.SubscriberKey, 
    Max(o.EventDate) AS LastOpenDate
FROM _Subscribers su
LEFT JOIN _Open o ON o.SubscriberKey = su.SubscriberKey
GROUP BY su.SubscriberKey
```

# Query 6: Cantidad de envios, fecha de ultimo envio y ultima apertura de cada subscriptor
```sql
SELECT 
    s.SubscriberKey, 
    COUNT(s.EventDate) AS Sents,
    MAX(s.EventDate) AS LastSentDate,
    MAX(o.EventDate) AS LastOpenDate
FROM _Sent s 
LEFT JOIN _Open o ON o.SubscriberKey = s.SubscriberKey AND o.JobID = s.JobID 
GROUP BY s.SubscriberKey
```
