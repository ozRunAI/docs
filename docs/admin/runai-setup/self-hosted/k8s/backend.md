
# Install the Run:ai Control Plane 

## Prerequisites and preperations

Make sure you have followed the Control Plane [prerequisites](./prerequisites.md) and [preperations](./preperations.md).

## Install the Control Plane

Run the helm command below:

=== "Connected"
    ``` bash
    helm repo add runai-backend https://runai.jfrog.io/artifactory/cp-charts-prod
    helm repo update
    helm upgrade -i runai-backend -n runai-backend runai-backend/control-plane --version "~2.17.0" \
        --set global.domain=<DOMAIN>  # (1)
    ```
    
    1. Domain name described [here](prerequisites.md#domain-name). 

    !!! Info
        To install a specific version, add `--version <version>` to the install command. You can find available versions by running `helm search repo -l runai-backend`.

=== "Airgapped"
    ``` bash
    helm upgrade -i runai-backend control-plane-<VERSION>.tgz  \ # (1)
        --set global.domain=<DOMAIN>  \ # (2)
        --set global.customCA.enabled=true \  # (3)
        -n runai-backend -f custom-env.yaml  # (4)
    ```
       
    1. Replace `<VERSION>` with the Run:ai control plane version.
    2. Domain name described [here](prerequisites.md#domain-name). 
    3. See the Local Certificate Authority instructions below
    4. `custom-env.yaml` should have been created by the _prepare installation_ script in the previous section. 

!!! Tip
    Use the  `--dry-run` flag to gain an understanding of what is being installed before the actual installation. 



### Additional configurations (optional)
There may be cases where you need to set additional properties as follows:

|  Key     | Change   | Description |
|----------|----------|-------------| 
| `redis.auth.password` | Redis (Runai internal cache mechanism) applicative password | Override the default password |
| `keycloakx.adminUser` | User name of the internal identity provider administrator | This user is the administrator of Keycloak | 
| `keycloakx.adminPassword` | Password of the internal identity provider administrator | This password is for the administrator of Keycloak | 
| `global.keycloakx.host` |  KeyCloak (Run:ai internal identity provider) host path | Override the DNS for Keycloak. This can be used to access Keycloak from outside the Run:ai Control Plane cluster via ingress | 
| `global.ingress.ingressClass` |  Ingress class  |  Run:ai default is using NGINX. If your cluster has a different ingress controller, you can configure the ingress class to be created by Run:ai |
| `global.ingress.tlsSecretName`  | TLS secret name  | Run:ai requires the creation of a secret with domain certificate. See [above](#domain-certificate). If the `runai-backend` namespace already had such a secret, you can set the secret name here  |
| `postgresql.enabled`| PostgreSQL installation | If set to `false` the PostgreSQL will not be installed |
| `global.postgresql.auth.host`  | PostgreSQL host | Hostname or IP address of the PostgreSQL server  |
| `global.postgresql.auth.port`  | PostgreSQL port | Port number on which PostgreSQL is running  |
| `global.postgresql.auth.username`  | PostgreSQL username | Username for connecting to PostgreSQL  |
| `global.postgresql.auth.password`  | PostgreSQL password | Password for the PostgreSQL user specified by `global.postgresql.auth.username`  |
| `global.postgresql.auth.postgresPassword`  | PostgreSQL default admin password | Password for the built-in PostgreSQL superuser (`postgres`)  |
| `postgresql.primary.initdb.password`  |  PostgreSQL default admin password    | Set the same password as in `global.postgresql.auth.postgresPassword` (if changed) |
| `grafana.dbUser`  | Grafana database username  | Username for accessing the Grafana database |
| `grafana.dbPassword`  | Grafana database password | Password for the Grafana database user |
| `grafana.adminUser`  | Grafana username  |   Override the Run:ai default user name for accessing Grafana |
| `grafana.adminPassword`  | Grafana password  |   Override the Run:ai default password for accessing Grafana |
| `thanos.receive.persistence.storageClass` and `postgresql.primary.persistence.storageClass` | Storage class | The installation to work with a specific storage class rather than the default one |
| `<component>` <br> &ensp;`resources:` <br> &emsp; `limits:` <br> &emsp; &ensp; `cpu: 500m` <br> &emsp; &ensp; `memory: 512Mi` <br> &emsp; `requests:` <br> &emsp; &ensp; `cpu: 250m` <br> &emsp; &ensp; `memory: 256Mi`  | Pod request and limits  |  `<component>` may be anyone of the following: `backend`, `frontend`, `assetsService`, `identityManager`, `tenantsManager`, `keycloakx`, `grafana`, `authorization`, `orgUnitService`,`policyService`  |   
|<div style="width:200px"></div>| | |

Use the `--set` syntax in the helm command above.  

#### External PostgreSQL database

If you have opted to connect to an [external PostgreSQL database](preperations.md#external-postgres-database-optional), refer to the additional configurations table below. Adjust the following parameters based on your connection details:

* `postgresql.enabled` - set to `false`
* `global.postgresql.auth.password`
* `global.postgresql.auth.username`
* `global.postgresql.auth.host`
* `global.postgresql.auth.port`
* `grafana.dbUser`
* `grafana.dbPassword`

## Next Steps

### Connect to Run:ai User interface

Go to: `runai.<domain>`. Log in using the default credentials: User: `test@run.ai`, Password: `Abcd!234`. Go to the Users area and change the password. 

### Enable Forgot Password (optional)

To support the *Forgot password* functionality, follow the steps below.

* Go to `runai.<domain>/auth` and Log in. 
* Under `Realm settings`, select the `Login` tab and enable the `Forgot password` feature.
* Under the `Email` tab, define an SMTP server, as explained [here](https://www.keycloak.org/docs/latest/server_admin/#_email){target=_blank}


### Install Run:ai Cluster
Continue with installing a [Run:ai Cluster](cluster.md).




