# Uncomment the lines marked with [*] if you want to export your rootCA.pem

FROM debian:buster-slim AS generate
WORKDIR /certificates

RUN apt-get update && apt-get install -y \
    curl \
    libnss3-tools \
    && rm -rf /var/lib/apt/lists/*

# Download mkcert
RUN curl -Lo mkcert https://dl.filippo.io/mkcert/latest?for=linux/amd64 && \
    chmod +x mkcert && mv mkcert /usr/local/bin/

# Install mkcert
RUN mkcert -install
RUN mkcert -key-file key.pem -cert-file cert.pem myapp.local localhost 127.0.0.1 ::1

# Copy the mkcert root certificate so we can transmit it to the host machine later
#[*] RUN cp $(mkcert -CAROOT)/rootCA.pem /certificates/rootCA-key.pem

FROM nginx:1.15.8-alpine
EXPOSE 443
COPY nginx.conf /etc/nginx/nginx.conf
# Copy of key and certificate
COPY --from=generate /certificates/key.pem /certificates/cert.pem  /etc/nginx/certs/

# Copy of root certificate
#[*] RUN mkdir -p /etc/nginx/certs/rootCA
#[*] COPY --from=generate /certificates/rootCA-key.pem  /etc/nginx/certs/rootCA/
