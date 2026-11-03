# Reporting API

Stateless FastAPI aggregation service for SparrowX. It calls `customer-api`, `task-api`, and `billing-api` inside the environment namespace and combines their responses into read-only reports.

The service exposes report routes under `/api/reports/`, `/health` for probes, `/api/reports/health` for smoke tests, `/metrics` for Prometheus, and `/docs`/`/redoc` for API documentation. Upstream URLs are configured with `CUSTOMER_API_URL`, `TASK_API_URL`, `BILLING_API_URL`, and `UPSTREAM_TIMEOUT_SECONDS`.

Argo CD deploys the dev and prod Helm values into separate namespaces on the same EKS cluster. The service does not own a database. Shared workflows build, scan, sign, promote, synchronize, and validate the image.

Define `HEALTH_CHECK_PATH` for deployment checks. Set repository-level `AWS_ROLE_NAME` only when overriding the organization’s default role; account, region, URLs, and Argo CD credentials are shared.

## Local development

```bash
python -m pip install -r requirements-dev.txt
pytest
uvicorn src.main:app --reload --port 8000
```

## License

See [LICENSE.md](LICENSE.md).
