# DockerMkcertForLocalSSL

This repository contains a basic implementation of [nginx Docker image](https://hub.docker.com/_/nginx) with [mkcert](https://github.com/FiloSottile/mkcert), a simple tool for making locally-trusted development certificates.

It removes the hassle of creating self-signed certificates that can cause trust errors on your browsers. Simply add the app.proxy configuration in your docker-compose.yml and copy the App.Proxy folder, and you're good to go !


## Installation

1. Copy/Paste the App.Proxy folder and its files into your project.
2. Edit your docker-compose.yml and copy/paste the app.proxy settings.
3. Rename some values located in docker-compose.yml and nginx.conf
4. Read these warnings carefully

> Remember that mkcert is meant for development purposes, not production, so it should not be used on end users' machines, and that you should not export or share rootCA-key.pem.

> **Warning**: the `rootCA-key.pem` file that mkcert automatically generates gives complete power to intercept secure requests from your machine. Do not share it.

## Trusting your local CA in your system

For all your future local certificates to be trusted, you need to import the root certificate on your host (local system) so they will be valid and recognized in your browsers. Please refer to the following installation section to get it done.

 ### Installation

1. Uncomment the lines marked with [*] in docker-compose.yml and Dockerfile.
2. Build the app.proxy container with docker-compose:
```bash
docker-compose up app.proxy --build
```
3. A **rootCA** folder is created containing your **rootCA-key.pem**

4. Install the root certificate on your host :

#### Windows
    1. Run mmc.exe 
    2. File > Add Snap-In > Certificates (computer account)
    3. Import rootCA-key.pem  
    4. Right-click on the "Trusted Root Certification Authorities" folder and select "All Tasks > Import"
#### macOS
    1. Open Keychain Access
    2. Add rootCA-key.pem 
    3. Mark it as approved
#### Linux
    1. Add it to /etc/ssl/certs/ 
    2. Update the certificates with:
    ```bash
        sudo cp rootCA-key.pem /usr/local/share/ca-certificates/
        sudo update-ca-certificates
    ```

5. Restart browser to make it effective


> **Warning**:  Once you imported the `rootCA-key.pem` in your host, re-comment/delete the lines marked with [*] in docker-compose.yml and Dockerfile.
