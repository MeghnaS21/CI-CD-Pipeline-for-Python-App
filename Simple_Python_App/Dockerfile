
FROM python:3.10-slim As Builder
WORKDIR /Simple_Python_App
COPY . .

FROM python:3.10-alpine
WORKDIR /Simple_Python_App
COPY --from=Builder /Simple_Python_App .

CMD [ "python", "app.py" ]
