FROM golang:1.13-alpine AS gobuild

RUN apk add git

ENV GO15VENDOREXPERIMENT=1
#Otherwise we get a breaking new version of cli
ENV GO111MODULE=on
RUN go get github.com/barnybug/cli53/cmd/cli53

FROM certbot/dns-route53
# Adapted from https://github.com/ebarault/letsencrypt-autorenew-docker
# 

VOLUME /var/log/letsencrypt /var/lib/letsencrypt /etc/letsencrypt

# Install dependencies
RUN apk add --no-cache curl bash nodejs jq dcron tini npm

# Install Heroku CLI
RUN npm install -g heroku

# Install Route53 CLI
COPY --from=gobuild /go/bin/cli53 /usr/local/bin

# Install scripts
COPY bin /usr/local/bin/

# Crontab specification
# See https://crontab.guru/ to test crontab expressions
#
#              |minute
#              |    |hour
#              |    |    |day of month
#              |    |    |    |month
#              |    |    |    |    |day of week
ENV CRON_FREQ="0    2    *    *    *"

ENTRYPOINT ["/usr/local/bin/entrypoint"]
