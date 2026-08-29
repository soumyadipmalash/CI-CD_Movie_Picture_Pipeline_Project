FROM python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

RUN useradd -m -u 1000 app

RUN apt-get update && \
    apt-get install -y \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir pipenv

RUN mkdir -p /var/www/app /app

COPY Pipfile* /app/

RUN cd /app && pipenv install --system --deploy

COPY . /app

RUN chown -R app:app /app /var/www/app

USER app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]