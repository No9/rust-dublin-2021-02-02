FROM quay.io/roche/default:1.4.0 as builder
COPY functions.rs /app-build/src
COPY lib.rs /app-build/src
COPY .env /app-build/src
RUN cargo build --release 
RUN cargo test --lib --release  
FROM quay.io/roche/alpine-libgcc:3.12
RUN addgroup -S rocheuser && adduser -S rocheuser -G rocheuser
WORKDIR "/app"
COPY --from=builder --chown=rocheuser /app-build/run.sh /app-build/Cargo.toml /app-build/target/release/roche-service ./
USER rocheuser
ENV PORT 8080
EXPOSE 8080
CMD ["./run.sh"]
