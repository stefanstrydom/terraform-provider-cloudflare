---
layout: "cloudflare"
page_title: "Cloudflare: cloudflare_access_application"
sidebar_current: "docs-cloudflare-resource-access-application"
description: |-
  Provides a Cloudflare Access Application resource.
---

# cloudflare_access_application

Provides a Cloudflare Access Application resource. Access Applications
are used to restrict access to a whole application using an
authorisation gateway managed by Cloudflare.

## Example Usage

```hcl
resource "cloudflare_access_application" "staging_app" {
  zone_id                   = "1d5fdc9e88c8a8c4518b068cd94331fe"
  name                      = "staging application"
  domain                    = "staging.example.com"
  session_duration          = "24h"
  auto_redirect_to_identity = false
}

# With CORS configuration
resource "cloudflare_access_application" "staging_app" {
  zone_id          = "1d5fdc9e88c8a8c4518b068cd94331fe"
  name             = "staging application"
  domain           = "staging.example.com"
  session_duration = "24h"
  cors_headers {
    allowed_methods = ["GET", "POST", "OPTIONS"]
    allowed_origins = ["https://example.com"]
    allow_credentials = true
    max_age = 10
  }
}
```

## Argument Reference

The following arguments are supported:

* `zone_id` - (Required) The DNS zone to which the access rule should be added.
* `name` - (Required) Friendly name of the Access Application.
* `domain` - (Required) The complete URL of the asset you wish to put
  Cloudflare Access in front of. Can include subdomains or paths. Or both.
* `session_duration` - (Optional) How often a user will be forced to
  re-authorise. Must be one of `30m`, `6h`, `12h`, `24h`, `168h`, `730h`.
* `cors_headers` - (Optional) CORS configuration for the Access Application. See
  below for reference structure.
* `allowed_idps` - (Optional) The identity providers selected for the application.


**cors_headers** allows the following:

* `allowed_methods` - (Optional) List of methods to expose via CORS.
* `allowed_origins` - (Optional) List of origins permitted to make CORS requests.
* `allowed_headers` - (Optional) List of HTTP headers to expose via CORS.
* `allow_all_methods` - (Optional) Boolean value to determine whether all
  methods are exposed.
* `allow_all_origins` - (Optional) Boolean value to determine whether all
  origins are permitted to make CORS requests.
* `allow_all_headers` - (Optional) Boolean value to determine whether all
  HTTP headers are exposed.
* `allow_credentials` - (Optional) Boolean value to determine if credentials
  (cookies, authorization headers, or TLS client certificates) are included with
  requests.
* `max_age` - (Optional) Integer representing the maximum time a preflight
  request will be cached.
* `auto_redirect_to_identity` - (Optional) Option to skip identity provider
  selection if only one is configured in allowed_idps. Defaults to `false`
  (disabled).
  
## Attributes Reference

The following additional attributes are exported:

* `id` - ID of the application
* `aud` - Application Audience (AUD) Tag of the application
* `domain` - Domain of the application
* `session_duration` - Length of session for the application before prompting for a sign in
* `auto_redirect_to_identity` - If the IdP selection page is skipped or not
* `allowed_idps` - The identity providers selected for the application

## Import

Access Applications can be imported using a composite ID formed of zone
ID and application ID.

```
$ terraform import cloudflare_access_application.staging cb029e245cfdd66dc8d2e570d5dd3322/d41d8cd98f00b204e9800998ecf8427e
```
