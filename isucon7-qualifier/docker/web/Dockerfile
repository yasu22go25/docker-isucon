FROM alpine AS build-dev

ARG GIT_URL=https://github.com/yasu22go25/isucon7-qualify.git

RUN \
  apk --no-cache add git wget unzip && \
  git clone --depth=1 $GIT_URL /isubata && \
  rm -rf /isubata/.git && \
  wget https://github.com/tkuchiki/alp/releases/download/v0.4.0/alp_linux_amd64.zip && \
  unzip alp_linux_amd64.zip && \
  mv alp /usr/local/bin/alp

FROM nginx:1.10

COPY --from=build-dev /isubata /home/isucon/isubata
#COPY --from=build-dev /isubata/files/app/nginx.conf /etc/nginx/conf.d/isucon.conf
COPY isucon.conf /etc/nginx/conf.d/isucon.conf

RUN \
  touch /etc/nginx/conf.d/log_format.conf && \
  chmod 777 /etc/nginx/conf.d/log_format.conf && \
  echo 'log_format ltsv "time:$time_local" \
                  "\thost:$remote_addr" \
                  "\tforwardedfor:$http_x_forwarded_for" \
                  "\treq:$request" \
                  "\tstatus:$status" \
                  "\tmethod:$request_method" \
                  "\turi:$request_uri" \
                  "\tsize:$body_bytes_sent" \
                  "\treferer:$http_referer" \
                  "\tua:$http_user_agent" \
                  "\treqtime:$request_time" \
                  "\tcache:$upstream_http_x_cache" \
                  "\truntime:$upstream_http_x_runtime" \
                  "\tapptime:$upstream_response_time" \
                  "\tvhost:$host"; \
                  access_log /var/log/nginx/access.log ltsv;' > /etc/nginx/conf.d/log_format.conf 
