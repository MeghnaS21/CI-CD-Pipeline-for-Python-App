FROM python:3.10-slim As builder

WORKDIR /build

COPY Requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

FROM python:3.10-alpine

WORKDIR /app

COPY --from=builder /usr/local/lib/python3.10/site-packages/ /usr/local/lib/python3.10/site-packages/
COPY --from=builder /build/ .

RUN adduser -D srerunner
USER srerunner

CMD ["python", "app.py"]


