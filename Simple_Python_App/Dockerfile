
FROM python:3.10-slim As Builder
WORKDIR /app
COPY . .

FROM python:3.10-alpine
WORKDIR /app
COPY --from=Builder /app .

CMD [ "python", "app.py" ]
