# Robot Shop Incident Response Runbook

## 1. Purpose
This runbook guides responders through investigating and recovering from Robot Shop availability and Kubernetes pod health alerts.

## 2. Alerts Covered
- `RobotShopPodsNotReady`
- `RobotShopAvailabilityProbeFailing`
- `RobotShopSLOBelowTarget`

## 3. Initial Checks

Check the application pods:

```bash
kubectl get pods -n robot-shop
```

Check deployments and services:

```bash
kubectl get deployments,services -n robot-shop
```

Check recent warning and error events:

```bash
kubectl get events -n robot-shop --sort-by=.lastTimestamp
```

## 4. Investigate the Problem

Inspect an unhealthy pod:

```bash
kubectl describe pod -n robot-shop <pod-name>
```

Check application logs:

```bash
kubectl logs -n robot-shop <pod-name> --tail=100
```

Check the availability probe in Prometheus by querying:

```promql
probe_success{job="robot-shop-availability"}
```

A value of `1` means the probe succeeded; `0` means it failed.

## 5. Mitigation and Recovery

If a pod is unhealthy, identify the cause before taking action. For a deployment managed by Kubernetes, a controlled restart may be appropriate:

```bash
kubectl rollout restart deployment/<deployment-name> -n robot-shop
```

Monitor recovery:

```bash
kubectl rollout status deployment/<deployment-name> -n robot-shop
kubectl get pods -n robot-shop
```

Confirm the availability probe returns `1` and review application logs for recurring errors.

Do not restart every service or delete persistent data as a first response.

## 6. Escalation and Communication

Review the firing alert in Alertmanager and the notification in Slack channel `#robot-shop-alerts`. Record the alert name, start time, observed impact, investigation findings, mitigation, and recovery time.

## 7. Post-Incident Review

Document the confirmed root cause, customer impact, timeline, recovery actions, and preventive improvements. If the root cause is unknown, state that it remains unconfirmed rather than guessing.

**Monitoring:** Prometheus, Grafana, Loki, Fluent Bit, Blackbox Exporter, and Alertmanager.

**SLO:** 99.5% availability over a rolling 30-day window.
