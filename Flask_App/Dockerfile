FROM python:3.10-slim

WORKDIR /app

COPY Requirements.txt

RUN pip install -r Requirements.txt

COPY . .

ENTRYPOINT ["flask.py"]

CMD ["python"]

