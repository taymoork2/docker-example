FROM nginx:latest

RUN DEBIAN_FRONTEND=noninteractive \
    apt-get update -qq && \
    apt-get -y install curl unzip runit vim && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -O https://releases.hashicorp.com/consul-template/0.12.0/consul-template_0.12.0_linux_amd64.zip
RUN unzip consul-template_0.12.0_linux_amd64.zip -d /usr/local/bin

ADD nginx.service /etc/service/nginx/run
ADD consul-template.service /etc/service/consul-template/run
ADD error_pages/* /usr/share/nginx/html/

RUN chmod +x /etc/service/nginx/run && chmod +x /etc/service/consul-template/run
RUN rm -v /etc/nginx/conf.d/*

ADD conf.d/*.conf /etc/consul-templates/
ADD nginx.conf /etc/consul-templates/nginx.conf
ADD nginx.conf /etc/nginx/nginx.conf

RUN rm -f consul-template_0.12.0_linux_amd64.zip

CMD ["/usr/bin/runsvdir", "/etc/service"]