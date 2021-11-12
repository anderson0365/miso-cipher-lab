FROM alpine:3.13.6
ENV FLASK_APP=/home/app/flaskr/app.py
ENV FLASK_DEBUG=1
ENV FLASK_ENV=development
COPY requirements.txt .
RUN apk update \
    && apk upgrade \
    && apk add --no-cache --virtual .build-deps musl-dev gcc\
    && apk add python3 python3-dev py3-pip \
    && ln -s /usr/bin/python3 /usr/bin/python \
    && pip install --no-cache-dir -r requirements.txt --ignore-installed six \
    && apk del .build-deps \
    && rm -rf requirements.txt \
    && addgroup app && adduser -S -G app app
USER app
RUN mkdir -p /home/app/flaskr
WORKDIR /home/app/flaskr
COPY --chown=app:app . .
CMD ["gunicorn", "-w", "1", "-b", "0.0.0.0:5000", "wsgi:app"]