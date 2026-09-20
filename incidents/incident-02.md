\# Incident 02: FTP Brute Force Activity (Port 21)



\*\*Detected by:\*\* \[rules/ftp\_bruteforce.yml](../rules/ftp\_bruteforce.yml)

\*\*Dataset:\*\* CICIDS2017 — Tuesday-WorkingHours

\*\*MITRE ATT\&CK:\*\* T1110 (Brute Force) — Credential Access



\## Summary

Analysis of the CICIDS2017 Tuesday dataset identified 7,937 flow records

labeled `FTP-Patator`, effectively all targeting destination port 21

(one anomalous outlier on port 80 excluded as noise). This matches the

known brute-force testing scenario built into the dataset.



\## Evidence



| Traffic Type | Count | Avg Flow Duration | Avg Fwd Packets |

|---|---|---|---|

| FTP-Patator (attack) | 7,937 | \~4,513,156 µs (\~4.5s) | 5.50 |

| Benign (port 21) | 1,059 | \~122,195 µs (\~0.12s) | 10.50 |



Splunk query used:

```

index=cicids2017 Label="FTP-Patator" | stats count avg("Flow Duration") avg("Total Fwd Packets") by "Destination Port"

```



\## Analysis

Attack flows to port 21 lasted roughly \*\*37x longer on average\*\* than

benign FTP traffic, while carrying fewer packets per flow. This is

consistent with repeated authentication attempts holding the connection

open across failed login tries, rather than genuine file transfer

activity. Benign sessions were short and packet-dense, consistent with

quick, successful authentication followed by actual data transfer.



\## Verdict

\*\*True Positive\*\* (confirmed via dataset ground-truth label `FTP-Patator`)



\## Notes

As with Incident 01, this dataset lacks source/destination IP fields,

so detection relies on port + duration + volume behavior. A production

rule would add per-source-IP thresholds to avoid flagging legitimate

long-lived automated FTP jobs.

