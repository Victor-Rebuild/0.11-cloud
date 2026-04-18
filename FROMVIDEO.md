# source.sh
## Goes in the repo root, replace the values with your actual ones
```
export TLS_CERT_PATH="/etc/letsencrypt/live/anki2.ca/fullchain.pem"
export TLS_KEY_PATH="/etc/letsencrypt/live/anki2.ca/privkey.pem"
export WEATHER_API_KEY="NO_KEY"
export VOSK_MODEL_PATH="$(pwd)/model"
export CHIPPER_PORT="442"
export WEB_PORT="3030"
```

# chipper-011.conf
## Goes in `/etc/apache2/sites-available/`, replace ServerName and certfiles with the actual paths
```
<VirtualHost *:443>
    ServerName oldvchipper-011.anki2.ca
    ServerAdmin webmaster@localhost

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    SSLEngine on
    SSLCertificateFile    /etc/letsencrypt/live/anki2.ca/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/anki2.ca/privkey.pem
    Include /etc/letsencrypt/options-ssl-apache.conf

    SSLProxyEngine on
    SSLProxyVerify none
    SSLProxyCheckPeerCN off
    SSLProxyCheckPeerName off

    ProxyPass / https://localhost:3030/
    ProxyPassReverse / https://localhost:3030/

    Header always set Access-Control-Allow-Origin "*"
    Header always set Access-Control-Allow-Methods "POST, GET, OPTIONS, DELETE, PUT"
    Header always set Access-Control-Allow-Headers "*"
    RewriteEngine On
    RewriteCond %{REQUEST_METHOD} OPTIONS
    RewriteRule ^(.*)$ $1 [R=200,L]
</VirtualHost>
```

# vosk api
## downloads and extracts into a folder named `vosk` inside the repo root
https://github.com/alphacep/vosk-api/releases/download/v0.3.45/vosk-linux-x86_64-0.3.45.zip

# vosk model
## extracts into a folder called `model` inside the repo root
https://alphacephei.com/vosk/models
