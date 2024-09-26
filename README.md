# php-docker
Base docker for php

```
cp .env.example .env
docker-compose up -d
```

## Access to app container to run bash
```
Root user: sudo docker-compose exec app bash
Alex user: sudo docker-compose exec --user=alex app bash
```
