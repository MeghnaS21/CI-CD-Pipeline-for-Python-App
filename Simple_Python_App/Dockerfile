# Stage 1: Build Stage
FROM python:3.10-slim AS builder
# Use a generic name like /build to avoid context confusion
WORKDIR /build
# This copies app.py from your folder into /build/app.py
COPY . . 

# Stage 2: Runtime Stage
FROM python:3.10-alpine
# Standard production directory
WORKDIR /app

# IMPORTANT: We copy the CONTENTS of /build into the current WORKDIR (./)
COPY --from=builder /build/ .

# SRE Best Practice: Run as non-root
RUN adduser -D srerunner
USER srerunner

# Since WORKDIR is /app, python looks for app.py right here
CMD ["python", "app.py"]
