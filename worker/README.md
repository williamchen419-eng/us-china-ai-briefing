# worker/

A Cloudflare Worker with two jobs: a Cron Trigger that dispatches the prefetch job on time, and the email sender that merges the day's report with the oracle's verdict into one HTML email. Not included in this edition.

If you build your own, keep every credential in the Worker's secret bindings, give each capability its own secret, and don't expose a general-purpose fetch proxy.
