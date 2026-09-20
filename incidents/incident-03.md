\# Incident 03: DoS Hulk Flood Activity (Port 80)



\*\*Detected by:\*\* \[rules/dos\_hulk.yml](../rules/dos\_hulk.yml)

\*\*Dataset:\*\* CICIDS2017 — Wednesday-WorkingHours

\*\*MITRE ATT\&CK:\*\* T1499 (Endpoint Denial of Service) — Impact



\## Summary

Analysis of the CICIDS2017 Wednesday dataset identified 231,073 flow

records labeled `DoS Hulk`, all targeting destination port 80. This is

the largest single attack category found in the dataset, consistent

with a high-volume HTTP flooding tool.



\## Evidence



| Traffic Type | Count | Avg Flow Duration | Avg Fwd Packets |

|---|---|---|---|

| DoS Hulk (attack) | 231,073 | \~57,081,732 µs (\~57.1s) | 5.28 |

| Benign (port 80) | 94,436 | \~27,735,307 µs (\~27.7s) | 57.4 |



Splunk query used:

```

index=cicids2017 Label="DoS Hulk" | stats count avg("Flow Duration") avg("Total Fwd Packets") by "Destination Port"

```



\## Analysis

Attack flows lasted roughly 2x longer than benign HTTP traffic on

average, but carried \*\*\~11x fewer packets per flow\*\*. This is

consistent with Hulk-style flooding: many rapid, low-content HTTP

requests designed to exhaust server resources, rather than genuine

sessions transferring real content. The disproportionate volume

(231k attack flows vs 94k benign, despite similar total port-80

traffic time) is itself a strong indicator of automated flooding.



\## Verdict

\*\*True Positive\*\* (confirmed via dataset ground-truth label `DoS Hulk`)



\## Notes

As with prior incidents, this dataset lacks source IP fields, so

detection is based on volume + packet-size anomalies rather than

per-source rate limiting. A production SOC would pair this with

web server access logs and source IP aggregation to confirm the

flood originates from a small number of hosts rather than

distributed legitimate traffic spikes.

