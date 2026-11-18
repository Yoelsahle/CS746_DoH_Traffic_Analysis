# Dataset information

**Original source:** CIRA-CIC-DoHBrw-2020, Canadian Institute for Cybersecurity, University of New Brunswick.

This repository analyzes two selected, pre-extracted CSV subsets:

| Included file | Notebook label | Original scenario |
| --- | --- | --- |
| `normal_doh_traffic_cloudflare_server (1).csv` | Benign | Normal Cloudflare DoH traffic |
| `dnscat2_data_4 (1).csv` | Malicious | DNSCat2-related tunneling traffic |

The notebooks rename *class labels*, not the original source or provenance. The supplied CSVs are pre-extracted feature tables, not raw PCAPs. No claim is made that the optional PCAP notebook reproduces these exact tables.

**Important limitations:** These two collection scenarios may differ in ways beyond maliciousness. Some numeric columns appear scaled; do not interpret them as physical units without verifying their transformation. Duplicates and class imbalance are reported rather than silently corrected. These data do not establish real-world intrusion detection performance.

**Redistribution:** Before publishing the included CSVs in a public repository, check the original dataset's redistribution terms. If redistribution is not permitted, remove the CSVs from the public repository and provide instructions for obtaining them from the original source. Keep the original attribution either way.
