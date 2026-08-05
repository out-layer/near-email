# Was pinned to 1.85 with a `cargo update home --precise 0.5.9` workaround, which existed only to
# hold a dependency back to something 1.85 could compile. The tree now pulls icu_* 2.2, which needs
# 1.86, so the workaround had become the thing breaking the build. Track stable, as db-api does.
FROM rust:bookworm AS builder

WORKDIR /app
COPY . .
# Build from the committed Cargo.lock: the deployed binary should match what was tested.
RUN cargo build --release --locked

FROM debian:bookworm-slim

RUN apt-get update && apt-get install -y \
    ca-certificates \
    libssl3 \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /app/target/release/smtp-server /usr/local/bin/

EXPOSE 25
CMD ["smtp-server"]
