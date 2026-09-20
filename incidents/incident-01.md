\# Incident 01: SSH Brute Force Activity (Port 22)



\*\*Detected by:\*\* \[rules/ssh\_bruteforce.yml](../rules/ssh\_bruteforce.yml)

\*\*Dataset:\*\* CICIDS2017 — Tuesday-WorkingHours

\*\*MITRE ATT\&CK:\*\* T1110 (Brute Force) — Credential Access



\## Summary

Analysis of the CICIDS2017 Tuesday dataset identified 5,897 flow records

labeled `SSH-Patator`, all targeting destination port 22. This matches

the known brute-force testing scenario built into the dataset.



\## Evidence



| Traffic Type | Count | Avg Flow Duration | Avg Fwd Packets |

|---|---|---|---|

| SSH-Patator (attack) | 5,897 | \~6,168,966 µs (\~6.2s) | 11.1 |

| Benign (port 22) | 2,136 | \~660,538 µs (\~0.66s) | 18.7 |



Splunk query used:

```

index=cicids2017 Label="SSH-Patator" | stats count avg("Flow Duration") avg("Total Fwd Packets") by "Destination Port"

```



\## Analysis

Attack flows to port 22 showed \*\*longer average duration\*\* than benign

traffic, despite carrying fewer packets per flow on average. This is

consistent with repeated authentication attempts holding a connection

open across multiple failed login tries, rather than a single fast

handshake. Benign SSH sessions on this dataset were shorter but more

data-dense, suggesting real usage (file transfer/interactive sessions)

versus repeated login probing.



\## Verdict

\*\*True Positive\*\* (confirmed via dataset ground-truth label `SSH-Patator`)



\## Notes

This dataset does not include source/destination IP fields (anonymized

in the MachineLearningCVE release), so detection here relies on

port + volume + duration behavior rather than IP reputation or

per-source thresholds. A production detection would add source IP

grouping to reduce false positives from legitimate high-frequency

SSH usage (e.g. CI/CD deploy keys).

