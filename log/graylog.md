# Graylog basic

### 1. Create docker-compose.yml
```
version: '2'
services:
  # MongoDB: https://hub.docker.com/_/mongo/
  mongodb:
    image: mongo:3
  # Elasticsearch: https://www.elastic.co/guide/en/elasticsearch/reference/5.6/docker.html
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:5.6.3
    environment:
      - http.host=0.0.0.0
      - transport.host=localhost
      - network.host=0.0.0.0
      # Disable X-Pack security: https://www.elastic.co/guide/en/elasticsearch/reference/5.6/security-settings.html#general-security-settings
      - xpack.security.enabled=false
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    mem_limit: 1g
  # Graylog: https://hub.docker.com/r/graylog/graylog/
  graylog:
    image: graylog/graylog:2.4.0-1
    environment:
      # CHANGE ME!
      - GRAYLOG_PASSWORD_SECRET=somepasswordpepper
      # Password: admin
      - GRAYLOG_ROOT_PASSWORD_SHA2=8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918
      - GRAYLOG_WEB_ENDPOINT_URI=http://127.0.0.1:9000/api
    links:
      - mongodb:mongo
      - elasticsearch
    depends_on:
      - mongodb
      - elasticsearch
    ports:
      # Graylog web interface and REST API
      - 9000:9000
      # Syslog TCP
      - 514:514
      # Syslog UDP
      - 514:514/udp
      # GELF TCP
      - 12201:12201
      # GELF UDP
      - 12201:12201/udp
```

## 2. Start docker compose
> cd path/graylog/contain/docker-compose-file
>
> docker-compose up -d

## 3. Access web
URL: `localhost:9000`\
Username: `admin`\
Pass: `admin`

## 4. Create inputs
> System -> Inputs -> Select input (GELF UDP) -> Launch new input -> Save

## 5. Write log in code (Golang)
- Use `https://github.com/sirupsen/logrus` for writing log in terminal
- Use `https://github.com/gemnasium/logrus-graylog-hook` for creating hook (write log and call graylog)
- Note:
	- use `127.0.0.1` instead of `localhost`
	- graylog UDP port: 12201
```
hook := graylog.NewGraylogHook("127.0.0.1:12201", map[string]interface{}{"this": "is logged every time"})
log.AddHook(hook)
```
- Test terminal
> echo '{"version": "1.1","host":"example.org","short_message":"A short message that helps you identify what is going on","full_message":"Backtrace here\n\nmore stuff","level":1,"_user_id":9001,"_some_info":"foo","_some_env_var":"bar"}' | gzip | nc -u -w 1 127.0.0.1 12201