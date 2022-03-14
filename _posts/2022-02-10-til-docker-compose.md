---
layout: post
title: TIL about docker-compose
---

Today I learned about docker-compose.

```bash
brew install docker
brew install docker-compose
```

`docker-compose.yml`
```yml
version: '2'

services:
  db:
    image: "postgres:10"
    environment:
      POSTGRES_DB: test
      POSTGRES_USER: test
      POSTGRES_PASSWORD: test
    ports:
      - "5432:5432"
```
