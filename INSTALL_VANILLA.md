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
* Signal server requires `X-Forwarded-For` header, otherwise it will index the NULL value with `.split`
```
curl -H "X-Forwarded-For: 127.0.0.1" -s 'http://localhost:8080/v1/accounts/sms/code/+13371337'
```