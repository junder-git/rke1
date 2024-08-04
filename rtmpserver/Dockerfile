FROM Ubuntu:22.04
RUN sudo apt update
RUN sudo apt install libnginx-mod-rtmp
RUN sudo ufw allow 1935,80,443/tcp
COPY nginx.conf /etc/nginx/nginx.conf
COPY /jserve /jserve
RUN sudo systemctl reload nginx.service
EXPOSE 80
