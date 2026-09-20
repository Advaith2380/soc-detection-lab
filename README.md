# \# SOC Detection Lab

# 

# A hands-on SOC analyst portfolio project: building and validating network

# intrusion detections against real, labeled attack traffic using Sigma

# rules and Splunk.

# 

# \## Overview

# 

# This project uses the \[CICIDS2017](https://www.unb.ca/cic/datasets/ids-2017.html)

# dataset (Canadian Institute for Cybersecurity, UNB) — labeled network

# flow data covering benign traffic and multiple real attack scenarios

# captured over five days. Each detection in this repo was built by:

# 

# 1\. Importing labeled flow data into Splunk

# 2\. Statistically comparing attack vs. benign traffic to find a

# &#x20;  distinguishing pattern

# 3\. Encoding that pattern as a vendor-agnostic \[Sigma](https://github.com/SigmaHQ/sigma) rule

# 4\. Writing up the finding as a SOC-style incident report, mapped to

# &#x20;  \[MITRE ATT\&CK](https://attack.mitre.org/)

# 

# \## Tools used

# 

# \- \*\*Splunk Enterprise\*\* (free tier) — SIEM for ingesting and querying flow data

# \- \*\*Sigma\*\* — vendor-agnostic detection rule format

# \- \*\*CICIDS2017\*\* — labeled network intrusion dataset (UNB)

# 

# \## Detections

# 

# | Rule | Attack Type | MITRE ATT\&CK | Writeup |

# |---|---|---|---|

# | \[ssh\_bruteforce.yml](rules/ssh\_bruteforce.yml) | SSH Brute Force (SSH-Patator) | T1110 – Credential Access | \[incident-01.md](incidents/incident-01.md) |

# | \[ftp\_bruteforce.yml](rules/ftp\_bruteforce.yml) | FTP Brute Force (FTP-Patator) | T1110 – Credential Access | \[incident-02.md](incidents/incident-02.md) |

# | \[dos\_hulk.yml](rules/dos\_hulk.yml) | DoS Hulk Flood | T1499 – Impact | \[incident-03.md](incidents/incident-03.md) |

# | \[port\_scan.yml](rules/port\_scan.yml) | Port Scan Reconnaissance | T1046 – Discovery | \[incident-04.md](incidents/incident-04.md) |

# 

# \## Repo structure

# 

# ```

# soc-detection-lab/

# ├── rules/        # Sigma detection rules (.yml)

# ├── incidents/    # SOC-style incident writeups per detection (.md)

# ├── data-notes/   # Notes on dataset structure and fields

# └── README.md

# ```

# 

# \## Notes

# 

# The `MachineLearningCVE` release of CICIDS2017 used here contains flow

# statistics (packet counts, duration, port, etc.) but no source/

# destination IP fields — these were anonymized in this release. As a

# result, detections in this repo are built on port, volume, and timing

# behavior rather than IP-based indicators. Each incident writeup notes

# how a production version of the rule would incorporate per-source-IP

# thresholds to reduce false positives.

# 

# Raw dataset files are not committed to this repo (see `.gitignore`) —

# download the dataset directly from

# \[UNB's CIC-IDS-2017 page](https://www.unb.ca/cic/datasets/ids-2017.html)

# to reproduce.

