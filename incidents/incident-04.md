\# Incident 04: Port Scan Reconnaissance Activity



\*\*Detected by:\*\* \[rules/port\_scan.yml](../rules/port\_scan.yml)

\*\*Dataset:\*\* CICIDS2017 — Friday-WorkingHours-Afternoon-PortScan

\*\*MITRE ATT\&CK:\*\* T1046 (Network Service Discovery) — Discovery



\## Summary

Analysis of the CICIDS2017 Friday afternoon dataset identified 158,930

flow records labeled `PortScan`, spread across at least 1,000 distinct

destination ports (top ports included 80, 21, 22, 443, 444, 139, 445,

1, 1000, 10000), each with near-identical low event counts.



\## Evidence



| Metric | Value |

|---|---|

| Total PortScan events | 158,930 |

| Distinct destination ports touched | 1,000 |

| Avg packets per flow | 1.02 |

| Avg flow duration | \~82,820 µs (\~0.08s) |



Splunk queries used:

```

index=cicids2017 Label="PortScan" | stats dc("Destination Port") as distinct\_ports, count by "Destination Port" | sort -count | head 10

```

```

index=cicids2017 Label="PortScan" | stats dc("Destination Port") as total\_distinct\_ports, avg("Total Fwd Packets") as avg\_packets, avg("Flow Duration") as avg\_duration

```



\## Analysis

Unlike the brute-force and DoS incidents in this repo, this activity

is characterized by \*\*breadth rather than volume on a single target\*\*.

Each flow carries roughly one packet and lasts under a tenth of a

second, consistent with a scanner rapidly probing port availability

across the target rather than sustaining a connection. The even

spread across 1,000 distinct ports (rather than concentration on one

or two) rules out a normal application communicating on a fixed port

set and instead points to automated reconnaissance tooling.



\## Verdict

\*\*True Positive\*\* (confirmed via dataset ground-truth label `PortScan`)



\## Notes

This dataset lacks source IP fields, so in a real environment this

detection would be paired with per-source-IP distinct-port-count

thresholds (the classic "N distinct ports from one IP within T

minutes" rule) to avoid flagging legitimate scanning by authorized

security tooling (e.g. internal vulnerability scans).

