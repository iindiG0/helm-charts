# helm-charts

Helm chart repository for the RKE2 lab.

| Chart | What it does |
|---|---|
| `cnpg-db` | One PostgreSQL cluster managed by CloudNativePG, labelled so Epinio can bind its credentials |

Repository URL: `https://raw.githubusercontent.com/iindiG0/helm-charts/main`

## Epinio catalog entry

`epinio/cnpg-catalog-service.yaml` adds "cnpg-postgres" to Epinio's service catalog.

```bash
kubectl apply -f epinio/cnpg-catalog-service.yaml
epinio service catalog            # cnpg-postgres should be listed
epinio service create cnpg-postgres mydb
epinio service bind mydb myapp
```

The app then finds its connection details as files:
`/configurations/<release>-app/uri` (also host, port, dbname, username, password).

## Updating the chart

Change files in `src/cnpg-db/`, bump `version` in `Chart.yaml`, then on a machine with Helm:

```bash
helm package src/cnpg-db
helm repo index . --url https://raw.githubusercontent.com/iindiG0/helm-charts/main
```

Commit the new `.tgz` and `index.yaml`.
