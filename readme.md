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
