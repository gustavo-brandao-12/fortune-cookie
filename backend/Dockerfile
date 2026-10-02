# Builder: imagem oficial do Rust sobre Debian 13 (trixie)
FROM docker.io/library/rust:1-slim-trixie AS builder

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        pkg-config \
        libssl-dev && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src/app

COPY Cargo.toml Cargo.lock ./
COPY src/ ./src/

RUN cargo build --release

# Runtime: mesma release do builder (OpenSSL 3)
FROM docker.io/library/debian:trixie-slim

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        libssl3t64 && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/log/fortune_backend

COPY --from=builder /usr/src/app/target/release/fortune_backend /usr/local/bin/fortune_backend

EXPOSE 8080

CMD ["fortune_backend"]