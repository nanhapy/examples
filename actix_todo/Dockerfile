# FROM rust:latest

# WORKDIR /usr/src/myapp

# COPY . .

# RUN cargo build --release

# RUN cargo install --path .

# CMD ["/usr/local/cargo/bin/actix-todo"]


# ------------------------------------------------------------------------------
# Cargo Build Stage
# ------------------------------------------------------------------------------

FROM rust:latest as cargo-build

WORKDIR /usr/src/myapp

COPY Cargo.toml Cargo.toml

COPY Cargo.lock Cargo.lock

RUN cargo build --release

RUN rm -f target/release/deps/actix-todo*

COPY . .

RUN cargo build --release

RUN cargo install --path .

# ------------------------------------------------------------------------------
# Final Stage
# ------------------------------------------------------------------------------

FROM alpine:latest

COPY --from=cargo-build /usr/local/cargo/bin/actix-todo /usr/local/bin/actix-todo

RUN diesel database setup

CMD ["actix-todo"]