# MLflow tracking server with Cloudflared and MinIO

## Setup

1. Clone this repository.
    ```bash
    git clone https://github.com/twbrandon7/Dockerized-MLflow-cloudflared-MinIO
    cd Dockerized-MLflow-Cloudflared-MinIO
    ```
2. Create a `.htpasswd` file for the authentication of MLflow.
    ```bash
    # Create the password for user `admin`.
    # The username and password will stored in `./nginx/.htpasswd`.
    htpasswd -c ./nginx/.htpasswd admin
    ```
3. Go to Cloudflare Zero Trust Dashboard, and create a tunnel ([see the documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/tunnel-guide/remote/#set-up-a-tunnel-remotely-dashboard-setup)). Once the tunnel created, you should see the following command appeared in the Dashboard:
    ```bash
    cloudflared service install <token>
    ```
    Copy the token, and it will be used later.
4. In Cloudflare Zero Trust Dashboard, configure `Public Hostname` of the tunnel. Add a `Public Hostname Page` and set `Service` to `http://nginx:80`.
    ![cloudflare-setting.png](./doc-images/cloudflare-setting.png)
5. Create a file called `.env` file in the same directory as `README.md`. Several options should be set in the `.env` file:
    - `TUNNEL_TOKEN`: The token created in step 3.
    - `MINIO_STORAGE_PATH`: The directory path to store the file of MinIO.
    - `MINIO_ACCESS_KEY`: The access key of MinIO. This can be used as username to to login MinIO console.
    - `MINIO_SECRET_KEY`: The secret key of MinIO. This can be used as password to login MinIO console.
    - `MLFLOW_STORAGE_PATH`: The directory path to store MLflow data.
    - `MLFLOW_S3_BUCKET_MAME`: The bucket in the MinIO. You can add a bucket by login to MinIO console. To access the console, please expose port `9001` of `minio` container (see [docker-compose.yml](./docker-compose.yml)).
    - `HTPASSWD_PATH`: The path of the password file created in step 2.

    For the example of the config file, please see [example.env](./example.env).

## Start MLflow

```bash
docker compose up -d
```
