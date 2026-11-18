FROM debian:trixie-slim

# Set up the OS
RUN apt-get update -y

RUN apt-get update && apt-get install -y --no-install-recommends \
        apache2 apache2-dev build-essential python3 python3-dev python3-venv python3-pip \
        libapache2-mod-wsgi-py3 \
        postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# Add our web app into the container
RUN mkdir -p /django/site/public/media /django/site/public/static /django/site/logs
COPY ./cs415/ /django/
COPY ./requirements.txt /django/
WORKDIR /django/

# Prepare Python
RUN python3 -m venv /django/env
RUN /django/env/bin/python3 -m pip install --upgrade pip
RUN /django/env/bin/python3 -m pip install -r requirements.txt
RUN /django/env/bin/python3 manage.py collectstatic

# Prepare and run Apache
RUN chown -R www-data:www-data /django/site
COPY ./cs415/000-default.conf /etc/apache2/sites-available/
RUN a2enmod wsgi

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
EXPOSE 80
