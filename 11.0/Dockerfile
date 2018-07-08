FROM alpine
# inoffeziell Docker MAINTAINER Patrick Weber <pat.weber91@gmail.com>
MAINTAINER Odoo S.A. <info@odoo.com>

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8
ENV CROSS_TRIPLE=aarch64-linux-gnu
COPY bin/* /usr/bin/
RUN set -x && \
        apk update && \
        apk add python3-dev nodejs git wget unzip linux-headers openldap-dev libjpeg postgresql-dev alpine-sdk libxml2-dev libxslt-dev  jpeg-dev zlib-dev freetype-dev lcms2-dev openjpeg-dev tiff-dev tk-dev tcl-dev harfbuzz-dev fribidi-dev && \
	apk add nodejs-npm || echo "npm in nodejs" && \
        pip3 install --upgrade pip && \
        npm config set unsafe-perm true && \
        npm install -g less && \
        wget https://github.com/odoo/odoo/archive/11.0.zip && \
        unzip *.zip -d /etc/odoo && \
        cd /etc/odoo && \
        mv */* . && \
        pip3 install -r requirements.txt && \
        apk del alpine-sdk git wget
        
COPY ./entrypoint.sh /
COPY ./odoo.conf /etc/odoo/
VOLUME ["/var/lib/odoo", "/mnt/extra-addons"]
RUN addgroup -S odoo && adduser -S odoo -G odoo && \
        chown odoo:odoo /etc/odoo/odoo.conf &&  \
	mkdir -p /mnt/extra-addons && \
	chown -R odoo:odoo /mnt/extra-addons

# Expose Odoo services
EXPOSE 8069 8071

# Set the default config file
ENV ODOO_RC /etc/odoo/odoo.conf

# Set default user when running the container
USER odoo

ENTRYPOINT ["/entrypoint.sh"]
CMD ["odoo"]
