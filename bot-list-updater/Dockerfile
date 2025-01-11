FROM rust:1-buster AS builder

RUN apt-get update && apt-get -y upgrade && apt-get -y install git openssl libssl-dev ca-certificates cmake

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

COPY . .

RUN cargo build --release --bin bot-list-updater

# Prod container
FROM debian:buster

RUN apt-get update && apt-get -y upgrade && apt-get -y install openssl libssl-dev ca-certificates

COPY --from=builder /tmp/compile/target/release/bot-list-updater /srv/bot-list-updater/bot-list-updater
RUN chmod +x /srv/bot-list-updater/bot-list-updater

RUN useradd -m container
USER container
WORKDIR /srv/bot-list-updater

CMD ["/srv/bot-list-updater/bot-list-updater"]