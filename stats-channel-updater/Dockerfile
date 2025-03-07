FROM rust:1-buster

RUN apt-get install -y apt-transport-https
RUN apt-get update && apt-get -y upgrade && apt-get -y install python3 openssl libssl-dev ca-certificates cmake

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

COPY . .

RUN cargo build --release --bin stats-channel-updater

FROM debian:buster

RUN apt-get update && apt-get -y upgrade && apt-get -y install openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/stats-channel-updater /srv/server_counter/server_counter
RUN chmod +x /srv/stats-channel-updater/stats-channel-updater

RUN useradd -m container
USER container
WORKDIR /srv/stats-channel-updater

CMD ["/srv/stats-channel-updater/stats-channel-updater"]