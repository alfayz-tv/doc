# security header
```
#1. HTTP Strict Transport Security (HSTS)
add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains; preload';

#2. Content Security Policy (CSP)
add_header Content-Security-Policy "default-src 'self'; font-src *;img-src * data:; script-src *; style-src *";

#3. X-XSS-Protection
add_header X-XSS-Protection "1; mode=block";

#4. X-Frame-Options
add_header X-Frame-Options "SAMEORIGIN";

#5. X-Content-Type-Options
add_header X-Content-Type-Options nosniff;

#6. Referrer-Policy
add_header Referrer-Policy "strict-origin";

#7. Permissions-Policy
add_header Permissions-Policy "geolocation=(),midi=(),sync-xhr=(),microphone=(),camera=(),magnetometer=(),gyroscope=(),fullscreen=(self),payment=()";

```