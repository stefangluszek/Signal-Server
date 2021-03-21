Vanilla Signal
==============

Installation
------------
1. Install postgresql.
2. Create signal user.
```
postgres=# create user signal with encrypted password 'signal';
```
3. Create databases.
```
postgres=# create database signal_abuse_db;
postgres=# grant all privileges on database signal_abuse_db to signal;

postgres=# create database signal_messages_db;
postgres=# grant all privileges on database signal_messages_db to signal;

postgres=# create database signal_accounts_db;
postgres=# grant all privileges on database signal_accounts_db to signal;
```

4. Migrate databases.
```
java -jar service/target/TextSecureServer-3.21.jar accountdb migrate service/config/config.yml
java -jar service/target/TextSecureServer-3.21.jar messagedb migrate service/config/config.yml
java -jar service/target/TextSecureServer-3.21.jar abusedb migrate service/config/config.yml
```

5. NOTES
* websocket endpoint: `ws://localhost:8080/v1/websocket`
* Register a new user
    ```
    curl -H "X-Forwarded-For: 127.0.0.1" -s 'http://localhost:8080/v1/accounts/sms/code/+13371337' \
    --next -X PUT http://localhost:8080/v1/accounts/code/133337 \
    -H 'Authorization: Basic KzEzMzcxMzM3OjEyMzQ1Njc4OTAxMjM0NTY=' \
    -d '{
            "registrationId": 1337,
            "supportSms": false,
            "signalingKey": "jMxcQdxZ8zyDbPd9p0A8eZa618kAgZTXF1Hz0T5qx6JEb21ewYD1k59PzCH7fZ1FBXbv7g==",
            "fetchesMessages": true
        }' \
    -H "Content-Type: application/json"
    ```
    TODO: To fully enable the device we need to register the prekeys with:
    ```
    PUT /v1/keys/
    ```