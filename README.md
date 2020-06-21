# Fotrino

This project is actively being developed and is not yet ready for use. Until I reach version 1.0, **updates might break existing installs** (eg. I won't provide DB migrations).

**Privacy Warning** The [URLs to access your photos](https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html) are valid for 24 hours and will work even if you are logged out. In other words, someone with access to your browser history/cache could access your photos.

## [Demo Site](https://www.fotrino.com/)

## Quickstart

This is the only repo you need to check out to run fotrino locally.

Note: You'll need [docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/) installed. You'll also need `sudo` to modify your hosts file.

**THIS IS SUPER INSECURE AND, AMONGST OTHER PROBLEMS, USES COMPLETELY MOCKED AUTHENTICATION.**

This quickstart will get a hacked together, non-production version of fotrino running locally (hopefully!). I do plan on providing cleaner and easier self-hosting options in the future.

#### Just do this stuff once!
```bash
# Generate a self signed certificate
# You can just press enter for all the questions
openssl req -new -newkey rsa:2048 -sha256 -days 3650 -nodes -x509 -keyout ./ssl/private.key -out ./ssl/public.crt -config ssl.conf
# Add some entries to your hosts file
echo "127.0.0.1   fotrino" | sudo tee -a /etc/hosts
echo "127.0.0.1   mock-oauth-server" | sudo tee -a /etc/hosts
echo "127.0.0.1   minio" | sudo tee -a /etc/hosts
```

#### Start Fotrino
```bash
# Start dependencies
docker-compose run --rm dependencies
# Start the service
docker-compose up -d fotrino-worker fotrino-backend fotrino-frontend nginx
```

**Now visit [https://fotrino:4443](https://fotrino:4443).**

#### Stop Fotrino
```bash
# Stop everything
docker-compose down
```

#### Troubleshooting

Your browser should complain that fotrino cannot be trusted.  You should visit each of these domains and accept the certificate.
- https://mock-oauth-server:4444
- https://minio:9000
- https://fotrino:4443

If you can't accept the certificate, try [this](https://medium.com/@dblazeski/chrome-bypass-net-err-cert-invalid-for-development-daefae43eb12).

#### Cleaning Up

If you'd like to trash all of your photos and DB you can just delete the `./minio` and `./postgres` folders.  They will be recreated the next time you start the services.

#### Updating
To update to the latest version of Fotrino just update the docker images. Updates are likely to **break your existing install**.  You should delete `./minio` and `./postgres` to avoid problems.
```bash
docker-compose pull
```

## Architecture

![fotrino architecture](https://docs.google.com/drawings/d/e/2PACX-1vSGRI9GP1OKkTt1A0YWXzWCZVZ5ZhtwJ7JMlOvahc-qVFVe9IGzvGr6aiKd4aj5_dNCXZlY3RFW_A95/pub?w=1440&h=1080)
