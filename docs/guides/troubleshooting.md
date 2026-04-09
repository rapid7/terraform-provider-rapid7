---
page_title: "Troubleshooting"
---

# Troubleshooting

## Network Request Logs

In the event of the provider encountering an API failure, a HAR file can be generated, capturing API network activity, which may be included as part of a bug report to the provider developers.

To enable generation of HAR files by the provider, set the `TF_PROVIDER_RAPID7_HAR_DIR` environment variable to a directory where you would like a HAR file to be created. The directory must exist, it will not be automatically created.

When HAR log recording is enabled a warning will be output when running Terraform, giving the path where the HAR file has been generated.

!> The generated HAR files will have API keys redacted from the recorded requests. It is recommended that you further review the content of the HAR file for any other information you may consider sensitive before sharing it.
