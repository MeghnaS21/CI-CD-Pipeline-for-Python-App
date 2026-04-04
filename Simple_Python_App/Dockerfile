
FROM python:3.10-slim As Builder
WORKDIR /build
COPY . .

FROM python:3.10-alpine
WORKDIR /app
COPY --from=Builder /build .

CMD [ "python", "app.py" ]
