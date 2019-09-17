FROM rust:latest as cargo-build

RUN apt-get update

RUN apt-get install musl-tools libpq-dev -y

RUN rustup target add x86_64-unknown-linux-musl

WORKDIR /usr/src/cc01

COPY ./Cargo.lock ./Cargo.lock
COPY ./Cargo.toml ./Cargo.toml

RUN mkdir src/

RUN echo 'fn main() { println!("Dummy") }' > ./src/main.rs

# RUN RUSTFLAGS=-Clinker=musl-gcc cargo build --release --target=x86_64-unknown-linux-musl

RUN cargo build --release

RUN rm -f target/release/deps/cc01*

COPY ./src ./src

RUN cargo install diesel_cli --no-default-features --features postgres

RUN cargo build --release

COPY migrations /app/cc01/migrations
COPY static /app/cc01/static
COPY templates /app/cc01/templates
RUN cp target/release/cc01 /app/cc01/cc01

WORKDIR /app/cc01

# CMD ["diesel database setup && ./cc01"]
CMD ["sh","-c","diesel database setup && ./cc01"]

# RUN RUSTFLAGS=-Clinker=musl-gcc cargo build --release --target=x86_64-unknown-linux-musl

# ------------------------------------------------------------------------------
# Final Stage
# ------------------------------------------------------------------------------

# FROM alpine:latest

# COPY --from=cargo-build /usr/src/cc01/target/x86_64-unknown-linux-musl/release/cc01 /usr/local/bin/cc01

# CMD ["/app/cc01/cc01"]