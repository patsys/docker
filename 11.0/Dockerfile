FROM aarch64/alpine
# inoffeziell Docker MAINTAINER Patrick Weber <pat.weber91@gmail.com>
MAINTAINER Odoo S.A. <info@odoo.com>

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x && \
        apk update && \
        apk add python3 nodejs git wget unzip && \
        pip3 install --upgrade pip && \
        npm config set unsafe-perm true && \
        npm install -g less && \
        wget https://github.com/odoo/odoo/archive/11.0.zip && \
        unzip *.zip -d /etc/odoo && \
        cd /etc/odoo && \
        mv */* . && \
        pip3 install -r requirements.txt
        
COPY ./entrypoint.sh /
COPY ./odoo.conf /etc/odoo/
RUN chown odoo /etc/odoo/odoo.conf

# Mount /var/lib/odoo to allow restoring filestore and /mnt/extra-addons for users addons
RUN mkdir -p /mnt/extra-addons \
        && chown -R odoo /mnt/extra-addons
VOLUME ["/var/lib/odoo", "/mnt/extra-addons"]

# Expose Odoo services
EXPOSE 8069 8071

# Set the default config file
ENV ODOO_RC /etc/odoo/odoo.conf

# Set default user when running the container
USER odoo

ENTRYPOINT ["/entrypoint.sh"]
CMD ["odoo"]
