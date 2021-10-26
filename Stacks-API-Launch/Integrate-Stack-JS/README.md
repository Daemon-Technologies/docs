---
sort: 2

---


# Integrate API with Stacks JS
In this document, we'll go over how to use [Stacks.js](https://github.com/blockstack/stacks.js) to connect the API Server we create in [last chapter](../Launch-API/README.md).

## Combine Your Stacks-API with Domain Name by Nginx
Https certification generation using [Certbot](https://certbot.eff.org/). Or you can buy your own domain name in Google Domain.
Here is the Nginx configuration:
```nginx
server {
        listen       80;
        server_name  <YOUR-OWN-DNS>;
        return 301    https://$host$request_uri;
}
server {
        server_name stacks-api.onstacks.com;

        error_page   500 502 503 504  /50x.html;
        location /{
            proxy_pass http://127.0.0.1:3999; ## You have to combine with 3999 port.
            proxy_http_version 1.1;
            proxy_set_header Host $proxy_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header REMOTE-HOST $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        listen 443 ssl; # managed by Certbot
        ssl on;
        ssl_certificate /etc/letsencrypt/live/<YOUR-OWN-DNS>/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/<YOUR-OWN-DNS>/privkey.pem; # managed by Certbot
}
```
Restart Nginx:
```bash
$ nginx -s reload
```

## Integrate Stack.js

```javascript
import { StacksMainnet, StacksTestnet, StacksMocknet } from '@stacks/network';
const network = new StacksMainnet();
network.coreApiUrl = 'https://<YOUR-OWN-DNS>/';
```

Check if network is mainnet:
```javascript
const isMainnet = network.isMainnet();
```

