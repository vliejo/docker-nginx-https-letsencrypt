# Overview of `nginx-https-letsencrypt`
This image will get certs from let's encrypt then adjust your nginx files.
It is expected to be extended, rather than run independently. Once the TLS certs
are obtained, the nginx config will be setup to proxy to a local server on port
3000.

The `secure_start` script is what gets the cert and configures nginx. It will
then execute the command you pass in (`npm start` in the below example).

When the image is run the domain to be secured must be provided as an
environment variable named `SECURE_DOMAIN_NAME` for `secure_start` to function.

# How to extend
Below is a basic node app running on port 3000. Adjust how you see fit:

```Dockerfile
FROM nginx-https-letsencrypt

# Setup Node app
WORKDIR /usr/src

COPY package.json app.js /usr/src/

RUN npm install

CMD /usr/local/bin/secure_start "npm start"
```

# Running the image
The domain to secure must be passed into the `SECURE_DOMAIN_NAME` environment
variable.

```
docker run -p 80:80 -p 443:443 -e SECURE_DOMAIN_NAME=mysecuresite.example.com <image_name>
```
