# DevOps Capstone Project 2 — Keep It Running

## Project: Stan's Robot Shop on Amazon EKS

### Completed components

| Component | Status |
|---|---|
| Robot Shop deployment on AWS EKS | Completed |
| Kubernetes application health and recovery | Completed |
| Prometheus monitoring | Completed |
| Grafana monitoring dashboard | Completed |
| Dashboard exported as JSON | Completed |
| Loki log aggregation | Completed |
| Fluent Bit log collection | Completed |
| Jaeger deployment | Completed |
| Synthetic trace demonstration | Completed |
| Blackbox availability monitoring | Completed |
| 99.5% availability SLO configuration | Completed |
| Prometheus alert rules | Completed |
| Alertmanager and Slack integration | Verified |
| Incident response runbook | Completed |
| SLO and error budget policy | Completed |
| Blameless incident postmortem | Completed |
| Controlled incident and recovery exercise | Completed |

### Outstanding improvements

1. Instrument an actual Robot Shop service with OpenTelemetry and verify a real application trace.
2. Add application-level HTTP request count, error rate and request latency metrics.
3. Independently verify the `RobotShopPodsNotReady` alert during a future incident exercise.
4. Accumulate sufficient probe history to assess the rolling 30-day SLO accurately.

### Incident exercise summary

The web deployment was deliberately scaled to zero replicas and subsequently restored to one replica. Kubernetes recreated the web pod successfully, and the deployment returned to 1/1 available.

### Project conclusion

The project demonstrates Kubernetes deployment and recovery, infrastructure monitoring, centralised logging, availability measurement, alert configuration and incident documentation.

The outstanding improvements are documented honestly and should be addressed before claiming complete application-level observability.
