# users
```sql
SELECT u.* FROM domains d 
JOIN contacts c ON (
d.registrant_id = c.id
OR d.admin_id = c.id
OR d.tech_id = c.id
OR d.billing_id = c.id
)
JOIN users u ON ( 
d.user_id = u.id
OR c.user_id = u.id )
WHERE d.zone = 'net.id'
AND exp_date > CURDATE()
GROUP BY id;

```

# domains
```sql
SELECT d.* FROM users u 
JOIN domains d ON
d.user_id = u.id
WHERE d.zone = 'net.id'
AND exp_date > CURDATE()
GROUP BY id

```

# contacts
```sql
SELECT c.* FROM domains d 
JOIN users u ON
d.user_id = u.id
JOIN contacts c ON 
c.user_id = u.id
OR
d.registrant_id = c.id
WHERE d.zone = 'net.id'
AND exp_date > CURDATE()
GROUP BY id;
```