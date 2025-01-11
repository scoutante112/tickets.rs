FROM rust:1-buster

RUN apt-get install -y apt-transport-https
RUN apt-get update && apt-get -y upgrade && apt-get -y install python3 openssl libssl-dev ca-certificates cmake

RUN mkdir -p /tmp/compile
WORKDIR /tmp/compile

COPY . .

RUN cargo build --release --bin server-counter

FROM debian:buster

RUN apt-get update && apt-get -y upgrade && apt-get -y install openssl libssl-dev ca-certificates

COPY --from=0 /tmp/compile/target/release/server-counter /srv/server-counter/server-counter
RUN chmod +x /srv/server-counter/server-counter

RUN useradd -m container
USER container
WORKDIR /srv/server-counter

CMD ["/srv/server-counter/server-counter"]