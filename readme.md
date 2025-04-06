# Homework 9
### Lev Zelenin

## Rest API for QR codes
Fixed the code and verified API works

## Non-docker server log
```
(venv) levz@LevsLaptop:~/projects/homework9$ ./start.sh
INFO:     Will watch for changes in these directories: ['/home/levz/projects/homework9']
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     Started reloader process [47768] using StatReload
INFO:     Started server process [47770]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
2025-04-06 11:43:36 - root - INFO - Creating QR code for URL: http://example.com/
2025-04-06 11:43:36 - root - INFO - QR code successfully saved to qr_codes/aHR0cDovL2V4YW1wbGUuY29tLw.png
INFO:     127.0.0.1:55536 - "POST /qr-codes/ HTTP/1.1" 201 Created
2025-04-06 11:43:50 - root - INFO - Listing all QR codes.
INFO:     127.0.0.1:58768 - "GET /qr-codes/ HTTP/1.1" 200 OK
^CINFO:     Shutting down
INFO:     Waiting for application shutdown.
INFO:     Application shutdown complete.
INFO:     Finished server process [47770]
INFO:     Stopping reloader process [47768]
(venv) levz@LevsLaptop:~/projects/homework9$ ls qr_codes/
aHR0cDovL2V4YW1wbGUuY29tLw.png
```

## Non-docker client log
```
(venv) levz@LevsLaptop:~/projects/homework9$ curl -X POST http://localhost:8000/qr-codes/ \
  -H "Authorization: Bearer secret" \
  -H "Content-Type: application/json" \
  -d '{"url":"http://example.com"}'
{"message":"QR code created successfully.","qr_code_url":"http://localhost/downloads/aHR0cDovL2V4YW1wbGUuY29tLw.png","links":[{"rel":"view","href":"http://localhost/downloads/aHR0cDovL2V4YW1wbGUuY29tLw.png","action":"GET","type":"image/png"},{"rel":"delete","href":"http://localhost/qr-codes/aHR0cDovL2V4YW1wbGUuY29tLw.png","action":"DELETE","type":"application/json"}]}
(venv) levz@LevsLaptop:~/projects/homework9$ curl -X GET http://localhost:8000/qr-codes/ -H "Authorization: Bearer secret"
[{"message":"QR code available","qr_code_url":"http://example.com/","links":[{"rel":"view","href":"http://localhost/downloads/aHR0cDovL2V4YW1wbGUuY29tLw.png","action":"GET","type":"image/png"},{"rel":"delete","href":"http://localhost/qr-codes/aHR0cDovL2V4YW1wbGUuY29tLw.png","action":"DELETE","type":"application/json"}]}]
```

## Docker server log
```
(venv) levz@LevsLaptop:~/projects/homework9$ docker-compose up --build
WARN[0000] /home/levz/projects/homework9/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
Compose can now delegate builds to bake for better performance.
 To do so, set COMPOSE_BAKE=true.
[+] Building 26.4s (16/16) FINISHED                                                      docker:default
 => [fastapi internal] load build definition from Dockerfile                                       0.0s
 => => transferring dockerfile: 1.61kB                                                             0.0s
 => WARN: FromAsCasing: 'as' and 'FROM' keywords' casing do not match (line 3)                     0.0s
 => [fastapi internal] load metadata for docker.io/library/python:3.12-slim-bullseye               0.7s
 => [fastapi auth] library/python:pull token for registry-1.docker.io                              0.0s
 => [fastapi internal] load .dockerignore                                                          0.0s
 => => transferring context: 75B                                                                   0.0s
 => [fastapi internal] load build context                                                          0.1s
 => => transferring context: 46.12kB                                                               0.1s
 => [fastapi 1/9] FROM docker.io/library/python:3.12-slim-bullseye@sha256:00ead89513b0a58e3805e7e  0.0s
 => => resolve docker.io/library/python:3.12-slim-bullseye@sha256:00ead89513b0a58e3805e7e88fc522a  0.0s
 => CACHED [fastapi 2/9] WORKDIR /myapp                                                            0.0s
 => CACHED [fastapi 3/9] RUN apt-get update     && apt-get install -y --no-install-recommends gcc  0.0s
 => [fastapi 4/9] COPY ./requirements.txt /myapp/requirements.txt                                  0.1s
 => [fastapi 5/9] RUN pip install --upgrade pip     && pip install -r requirements.txt            19.3s
 => [fastapi 6/9] COPY . /myapp                                                                    0.2s
 => [fastapi 7/9] COPY start.sh /start.sh                                                          0.1s
 => [fastapi 8/9] RUN chmod +x /start.sh                                                           0.4s
 => [fastapi 9/9] RUN useradd -m myuser                                                            0.6s
 => [fastapi] exporting to image                                                                   4.6s
 => => exporting layers                                                                            3.0s
 => => exporting manifest sha256:d22ce4a3caa6bddcd948b717f8aacb1cd9f7803b795ecbffad56b8fd5e39a6c6  0.0s
 => => exporting config sha256:3003837d36a7c92395542a429e82bce22ec90636ff554697ea915ad0b9e7d3f1    0.0s
 => => exporting attestation manifest sha256:144a2e121cd06a99ed555092ab643ccbabdd5428c9c01ac32ca1  0.0s
 => => exporting manifest list sha256:8722206600491deb7fc2448744fe63e0e41ec8e319dee4db8cd7d287d08  0.0s
 => => naming to docker.io/library/homework9-fastapi:latest                                        0.0s
 => => unpacking to docker.io/library/homework9-fastapi:latest                                     1.4s
 => [fastapi] resolving provenance for metadata file                                               0.0s
[+] Running 3/3
 ✔ fastapi                        Built                                                            0.0s
 ✔ Container homework9-fastapi-1  Recreated                                                        0.7s
 ✔ Container homework9-nginx-1    Created                                                          0.0s
Attaching to fastapi-1, nginx-1
nginx-1    | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
nginx-1    | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
nginx-1    | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
nginx-1    | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
nginx-1    | 10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
nginx-1    | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
nginx-1    | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
nginx-1    | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
nginx-1    | /docker-entrypoint.sh: Configuration complete; ready for start up
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: using the "epoll" event method
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: nginx/1.27.4
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: OS: Linux 5.15.167.4-microsoft-standard-WSL2
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker processes
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 28
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 29
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 30
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 31
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 32
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 33
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 34
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 35
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 36
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 37
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 38
nginx-1    | 2025/04/06 16:41:29 [notice] 1#1: start worker process 39
fastapi-1  | INFO:     Will watch for changes in these directories: ['/myapp']
fastapi-1  | INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
fastapi-1  | INFO:     Started reloader process [7] using StatReload
fastapi-1  | INFO:     Started server process [9]
fastapi-1  | INFO:     Waiting for application startup.
fastapi-1  | INFO:     Application startup complete.
fastapi-1  | 2025-04-06 16:41:56 - root - INFO - Creating QR code for URL: http://example.com/
fastapi-1  | 2025-04-06 16:41:56 - root - INFO - QR code successfully saved to qr_codes/aHR0cDovL2V4YW1wbGUuY29tLw.png
fastapi-1  | INFO:     172.18.0.2:55986 - "POST /qr-codes/ HTTP/1.0" 201 Created
nginx-1    | 172.18.0.1 - - [06/Apr/2025:16:41:56 +0000] "POST /qr-codes/ HTTP/1.1" 201 370 "-" "curl/8.5.0" "-"
fastapi-1  | 2025-04-06 16:42:11 - root - INFO - Listing all QR codes.
fastapi-1  | INFO:     172.18.0.2:60794 - "GET /qr-codes/ HTTP/1.0" 200 OK
nginx-1    | 172.18.0.1 - - [06/Apr/2025:16:42:11 +0000] "GET /qr-codes/ HTTP/1.1" 200 322 "-" "curl/8.5.0" "-"
Gracefully stopping... (press Ctrl+C again to force)
[+] Stopping 2/2
 ✔ Container homework9-fastapi-1  Stopped                                                         10.4s
 ✔ Container homework9-nginx-1    Stopped                                                          0.8s
```

## Docker client log
```
(venv) levz@LevsLaptop:~/projects/homework9$ ls qr_codes/
(venv) levz@LevsLaptop:~/projects/homework9$ curl -X POST http://localhost/qr-codes/ \
  -H "Authorization: Bearer secret" \
  -H "Content-Type: application/json" \
  -d '{"url":"http://example.com"}'
{"message":"QR code created successfully.","qr_code_url":"http://localhost/downloads/aHR0cDovL2V4YW1wbGUuY29tLw.png","links":[{"rel":"view","href":"http://localhost/downloads/aHR0cDovL2V4YW1wbGUuY29tLw.png","action":"GET","type":"image/png"},{"rel":"delete","href":"http://localhost/qr-codes/aHR0cDovL2V4YW1wbGUuY29tLw.png","action":"DELETE","type":"application/json"}]}

(venv) levz@LevsLaptop:~/projects/homework9$ ls qr_codes/
aHR0cDovL2V4YW1wbGUuY29tLw.png

(venv) levz@LevsLaptop:~/projects/homework9$ curl -X GET http://localhost/qr-codes/ \
  -H "Authorization: Bearer secret"
[{"message":"QR code available","qr_code_url":"http://example.com/","links":[{"rel":"view","href":"http://localhost/downloads/aHR0cDovL2V4YW1wbGUuY29tLw.png","action":"GET","type":"image/png"},{"rel":"delete","href":"http://localhost/qr-codes/aHR0cDovL2V4YW1wbGUuY29tLw.png","action":"DELETE","type":"application/json"}]}]
```
