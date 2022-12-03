# Api Powerdns
> ## all request must add headers **X-API-Key**

# List Zones
```sh
GET {{url}}/api/v1/servers/localhost/zones
```

# Create Domain
```sh
POST {{url}}/api/v1/servers/localhost/zones

# with body
{
    "name": "yourdomain.id.",
    "kind": "Native", //Master or Slave
    "masters": [],
    "nameservers": []
}

```

# Detail Domain
```sh
GET {{url}}/api/v1/servers/localhost/zones/yourdomain.id.
```

# Add Record
```sh
PATCH {{url}}/api/v1/servers/localhost/zones/yourdomain.id.

# with body
{
    "rrsets": [
        {
            "name": "yourdomain.id.",
            "type": "A",
            "ttl": 3600,
            "changetype": "REPLACE",
            "records": [
                {
                    "content": "45.126.59.45",
                    "disabled": false
                }
            ]
        },
        {
            "name": "api.yourdomain.id.",
            "type": "A",
            "ttl": 3600,
            "changetype": "REPLACE",
            "records": [
                {
                    "content": "45.126.59.45",
                    "disabled": false
                }
            ]
        },
        //subdomain
        {
            "name": "mesh.yourdomain.id.",
            "type": "A",
            "ttl": 3600,
            "changetype": "REPLACE",
            "records": [
                {
                    "content": "45.126.59.45",
                    "disabled": false
                }
            ]
        },
        //subdomain
        {
            "name": "rmm.yourdomain.id.",
            "type": "A",
            "ttl": 3600,
            "changetype": "REPLACE",
            "records": [
                {
                    "content": "45.126.59.45",
                    "disabled": false
                }
            ]
        }
    ]
}
```

# Delete Domain
```sh
DELETE {{url}}/api/v1/servers/localhost/zones/yourdomain.id.

```


# Add Cname
```sh
PATCH {{url}}/api/v1/servers/localhost/zones/yourdomain.id.

# with body
{
    "rrsets": [
        {
            "name": "_D5A48FD7972C2C087DC46B0EE3AED132.yourdomain.id.",
            "type": "CNAME",
            "ttl": 60,
            "changetype": "REPLACE",
            "records": [
                {
                    "content": "D6855F9507F7758EBA424418CEB64194.156D336BF87514254BC47705CBDD4B90.898d75ec0442155.comodoca.com.",
                    "disabled": false
                }
            ]
        }
    ]
}
```

# Add MX
```sh
PATCH {{url}}/api/v1/servers/localhost/zones/yourdomain.id.

# with body
{
	"rrsets": 
		[
			{
				"name": "yourdomain.id.",
			   "type": "MX",
			   "ttl": 3600,
			   "changetype": "REPLACE",
			   "records": 
					[ 
						{
							"content": "10 mail.yourdomain.id.", 
							"disabled": false
						}
					]
			}
		] 
}
```

# Add Txt Record
```sh
PATCH {{url}}/api/v1/servers/localhost/zones/yourdomain.id.

# with body
{
    "rrsets":[
        {
            "name":"_acme-challenge.yourdomain.id.",
            "type":"TXT",
            "ttl":60,
            "changetype":"REPLACE",
            "records":[
                {
                    "content":"\"PoS0Q5tAIE5nxH953SkBN7dYdQZX57J3pjBYLCpFX1U\"",
                    "set_ptr":false,
                    "disabled":false
                }
            ]
        }
    ]
}
```

# Update NS
```sh
PATCH {{url}}/api/v1/servers/localhost/zones/yourdomain.id.

# with body
{
	"rrsets": 
		[
			{
				"name": "yourdomain.id.",
			   "type": "NS",
			   "ttl": 60,
			   "changetype": "REPLACE",
			   "records": 
					[ 
						{
							"content": "nsbikin.domain.id.", 
							"disabled": false
						}
					]
			}
		] 
}
```

# Update SOA
```sh
PATCH {{url}}/api/v1/servers/localhost/zones/yourdomain.id.

# with body
{
	"rrsets": 
		[
			{
				"name": "yourdomain.id.",
			   "type": "SOA",
			   "ttl": 3600,
			   "changetype": "REPLACE",
			   "records": 
					[ 
						{
							"content": "dns.tdihost.id. hostmaster.tdihost.id. 2022012203 10800 3600 604800 3600", 
							"disabled": false
						}
					]
			}
		] 
}
```