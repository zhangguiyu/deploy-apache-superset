# Deploy Apache-superset with existing Postgresql Instance

## Prerequisites

* A Postgresql Instance deployed on same kubernetes cluster
* Helm installed in system & basic knowledge of Helm.
* Admin username & password of postgres instance
* Superset Database created in postgresql (If not then follow step 3)
* **Optionally** Cert-manager if you want to have SSL certificates on hostnames. 


## Directory Structure

```bash
.
├── apache-superset
│   ├── Chart.lock
│   ├── charts
│   ├── Chart.yaml
│   ├── custom-values.yaml
│   ├── templates
│   ├── values.schema.json
│   └── values.yaml
└── README.md

3 directories, 6 files
```

## Usage

### 1. Clone the repo

```bash
git clone https://github.com/knoldus/deploy-apache-superset.git
cd deploy-apache-superset
```

### 2. Create or Edit apache-superset/custom-values.yaml file

Replace placeholders with username, password and service name.

```yaml
supersetNode:
  connections:
    ## Changes to service name of your postgresql instance
    db_host: <POSTGRES_SERVICE_NAME>.<NAMESPACE>.svc.cluster.local
    db_port: "5432"
    ## change the <USERNAME> with admin username of postgres 
    db_user: <USERNAME>
    ## change the <PASSWORD> with admin username of postgres
    db_pass: <PASSWORD>
    db_name: superset
```


### 3. Create database in postgresql **(If not created)**

```bash
kubectl port-forward service/<SERVICE_NAME_OF_POSTGRES> 5432
createdb -h 127.0.0.1 -p 5432 -U <USERNAME> superset
```


### 4. Deploy Apache superset


```bash
helm upgrade --install superset apache-superset -f apache-superset/custom-values.yaml --namespace superset --create-namespace
```
