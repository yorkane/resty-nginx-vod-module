# Dockerfile - alpine
# https://github.com/openresty/docker-openresty
FROM alpine:3.12.0
RUN apk add --no-cache \
        curl zlib\
        perl \
		ffmpeg ffmpeg-dev \
		xxhash xxhash-dev \
		vips \
		libqrencode-dev

COPY openresty/ /usr/local/openresty/
# Add additional binaries into PATH for convenience
ENV PATH=$PATH:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin

CMD ["/usr/local/openresty/bin/openresty", "-g", "daemon off;"]

# Use SIGQUIT instead of default SIGTERM to cleanly drain requests
STOPSIGNAL SIGQUIT

# 


# docker build ./ -t rvod:1
# docker run -d -v $PWD:/data -p 3080:80 --name r1 rvod:1