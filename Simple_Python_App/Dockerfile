# Stage 1: Build
FROM python:3.10-slim AS builder
WORKDIR /build
# Because the context is now the folder, app.py is right here!
COPY . .

# Stage 2: Runtime
FROM python:3.10-alpine
WORKDIR /app
# Copy from the builder's /build/ into the current /app/
COPY --from=builder /build/ .

# Security best practice
RUN adduser -D srerunner
USER srerunner

CMD ["python", "app.py"]
