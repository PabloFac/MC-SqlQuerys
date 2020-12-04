## Advanced Querys

# Query 1
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

# Query 2
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

# Query 3
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


