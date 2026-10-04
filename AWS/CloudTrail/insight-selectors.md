# Insight Selectors

> Insight Selectors. Part of the [CloudTrail](../) cheatsheet.

Insights are CloudTrail's own anomaly detection. They write summary events into
the trail and event history so you see the spike without reading every record.

## To turn on call rate and error rate insights

The following example enables both insight types, which is what surfaces
"An API call rate is spiking" notifications.

```bash
aws cloudtrail put-insight-selectors \
    --trail-name <trail> \
    --insight-selectors file://insight-selectors.json
```

`insight-selectors.json`:

```json
[
  { "InsightType": "ApiCallRateInsight" },
  { "InsightType": "ApiErrorRateInsight" }
]
```

## To check which insights are on

The following example returns the insight selectors for a trail.

```bash
aws cloudtrail get-insight-selectors --trail-name <trail>
```
