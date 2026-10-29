# Robot Shop — SLO Policy and Incident Postmortem

## Part A: Service Level Objective (SLO) and Error Budget Policy

### Availability objective

Robot Shop targets **99.5% availability over a rolling 30-day period**.

The permitted error budget is 0.5%, equivalent to approximately 3 hours and 36 minutes of unavailability in a 30-day period.

### Measurement and monitoring

Availability is monitored using Blackbox Exporter and Prometheus. Grafana provides monitoring dashboards, while Alertmanager sends notifications to the `#robot-shop-alerts` Slack channel.

### Error budget policy

| Remaining error budget | Operational response |
|---|---|
| Above 50% | Normal deployments may continue. |
| 25–50% | Review risky deployments and changes. |
| Below 25% | Prioritise reliability improvements. |
| Exhausted | Pause non-essential releases until reliability improves. |

The 30-day SLO should be assessed using sufficient probe history. An alert indicating that the target is below threshold does not, by itself, establish a sustained outage.

---

## Part B: Blameless Incident Postmortem

### Incident title
Robot Shop Web Availability Drill

### Incident type
Controlled availability incident simulation.

### Objective
Test the monitoring and recovery process by deliberately reducing the web deployment to zero replicas.

### Incident timeline

1. The Robot Shop web deployment was scaled from its normal replica count to zero.
2. Kubernetes reported the web deployment as `0/0`.
3. The other listed application services remained running.
4. The web deployment was restored to one replica.
5. Kubernetes created a replacement web pod.
6. The replacement pod reached `1/1 Running`, and the deployment returned to `1/1 Available`.

### Root cause

The incident was intentionally introduced by scaling the web deployment to zero replicas during a controlled operational drill. This was not an unexpected production failure.

### Impact

The web deployment had no available replicas during the drill. The exact duration of user-facing unavailability and external customer impact were not measured.

### Resolution

The deployment was scaled back to one replica. Kubernetes successfully recreated the web pod and restored the deployment.

### Monitoring observations

- Prometheus availability and SLO alert rules were configured.
- Alertmanager had a Slack receiver configured for `#robot-shop-alerts`.
- The specific `RobotShopPodsNotReady` alert was not independently verified during this drill.
- The availability and SLO alerts were visible in Alertmanager.
- The 99.5% SLO should not be considered achieved until sufficient measurement history is available.

### Lessons learned and improvements

1. Verify alert evaluation and notification delivery during future incident exercises.
2. Add application-level request, error-rate and latency instrumentation.
3. Instrument an actual Robot Shop service with OpenTelemetry.
4. Maintain and periodically test the incident response runbook.
5. Monitor error-budget consumption before risky deployments.

### Final status

**Service recovered successfully.** The controlled drill demonstrated Kubernetes recovery, while alert verification and additional application-level observability remain improvement items.

