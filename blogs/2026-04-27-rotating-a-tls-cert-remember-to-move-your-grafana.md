---
title: "Rotating a TLS Cert? Remember to Move Your Grafana Datasource Versions Forward"
url: "https://docs.runwhen.com/blog/grafana-datasource-versions-forward/"
date: "2026-04-27"
feed_url: "https://docs.runwhen.com/blog/rss.xml"
---
A TLS and false-positive alert story. Grafana provisioned datasources won't reload a rotated cert until you nudge the YAML `version` integer forward. We forgot.
