<!-- This file was automatically generated by the `build-harness`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

[![Cloud Posse](https://cloudposse.com/logo-300x69.svg)](https://cloudposse.com)

# terraform-aws-cloudfront-cdn [![Build Status](https://travis-ci.org/cloudposse/terraform-aws-cloudfront-cdn.svg?branch=master)](https://travis-ci.org/cloudposse/terraform-aws-cloudfront-cdn) [![Latest Release](https://img.shields.io/github/release/cloudposse/terraform-aws-cloudfront-cdn.svg)](https://github.com/cloudposse/terraform-aws-cloudfront-cdn/releases/latest) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Terraform Module that implements a CloudFront Distribution (CDN) for a custom origin (e.g. website) and [ships logs to a bucket](https://github.com/cloudposse/terraform-aws-log-storage). 

If you need to accelerate an S3 bucket, we suggest using [`terraform-aws-cloudfront-s3-cdn`](https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn) instead.


---

This project is part of our comprehensive ["SweetOps"](https://docs.cloudposse.com) approach towards DevOps. 


It's 100% Open Source and licensed under the [APACHE2](LICENSE).





## Usage

Basic usage:

```hcl
module "cdn" {
  source             = "git::https://github.com/cloudposse/terraform-aws-cloudfront-cdn.git?ref=master"
  namespace          = "cp"
  stage              = "prod"
  name               = "app"
  aliases            = ["cloudposse.com", "www.cloudposse.com"]
  parent_zone_name   = "cloudposse.com"
  origin_domain_name = "origin.cloudposse.com"
}
```


Complete example of setting up CloudFront Distribution with Cache Behaviors for a WordPress site: [`examples/wordpress`](examples/wordpress/main.tf)


### Generating ACM Certificate

Use the AWS cli to [request new ACM certifiates](http://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request.html) (requires email validation)
```
aws acm request-certificate --domain-name example.com --subject-alternative-names a.example.com b.example.com *.c.example.com
```






## Makefile Targets
```
Available targets:

  help                                This help screen
  help/all                            Display help for all targets
  lint                                Lint terraform code

```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| acm_certificate_arn | Existing ACM Certificate ARN | string | `` | no |
| aliases | List of aliases. CAUTION! Names MUSTN'T contain trailing `.` | list | `<list>` | no |
| allowed_methods | List of allowed methods (e.g. ` GET, PUT, POST, DELETE, HEAD`) for AWS CloudFront | list | `<list>` | no |
| attributes | Additional attributes (e.g. `policy` or `role`) | list | `<list>` | no |
| cache_behavior | List of cache behaviors to implement | list | `<list>` | no |
| cached_methods | List of cached methods (e.g. ` GET, PUT, POST, DELETE, HEAD`) | list | `<list>` | no |
| comment | Comment for the origin access identity | string | `Managed by Terraform` | no |
| compress | (Optional) Whether you want CloudFront to automatically compress content for web requests that include Accept-Encoding: gzip in the request header (default: false) | string | `false` | no |
| custom_error_response | (Optional) - List of one or more custom error response element maps | list | `<list>` | no |
| default_root_object | Object that CloudFront return when requests the root URL | string | `index.html` | no |
| default_ttl | Default amount of time (in seconds) that an object is in a CloudFront cache | string | `60` | no |
| delimiter | Delimiter to be used between `name`, `namespace`, `stage`, etc. | string | `-` | no |
| enabled | Set to false to prevent the module from creating any resources | string | `true` | no |
| forward_cookies | Specifies whether you want CloudFront to forward cookies to the origin. Valid options are all, none or whitelist | string | `none` | no |
| forward_cookies_whitelisted_names | List of forwarded cookie names | list | `<list>` | no |
| forward_headers | Specifies the Headers, if any, that you want CloudFront to vary upon for this cache behavior. Specify `*` to include all headers. | list | `<list>` | no |
| forward_query_string | Forward query strings to the origin that is associated with this cache behavior | string | `false` | no |
| geo_restriction_locations | List of country codes for which  CloudFront either to distribute content (whitelist) or not distribute your content (blacklist) | list | `<list>` | no |
| geo_restriction_type | Method that use to restrict distribution of your content by country: `none`, `whitelist`, or `blacklist` | string | `none` | no |
| is_ipv6_enabled | State of CloudFront IPv6 | string | `true` | no |
| log_expiration_days | Number of days after which to expunge the objects | string | `90` | no |
| log_glacier_transition_days | Number of days after which to move the data to the glacier storage tier | string | `60` | no |
| log_include_cookies | Include cookies in access logs | string | `false` | no |
| log_prefix | Path of logs in S3 bucket | string | `` | no |
| log_standard_transition_days | Number of days to persist in the standard storage tier before moving to the glacier tier | string | `30` | no |
| max_ttl | Maximum amount of time (in seconds) that an object is in a CloudFront cache | string | `31536000` | no |
| min_ttl | Minimum amount of time that you want objects to stay in CloudFront caches | string | `0` | no |
| name | Name  (e.g. `bastion` or `db`) | string | - | yes |
| namespace | Namespace (e.g. `cp` or `cloudposse`) | string | - | yes |
| origin_domain_name | (Required) - The DNS domain name of your custom origin (e.g. website) | string | `` | no |
| origin_http_port | (Required) - The HTTP port the custom origin listens on | string | `80` | no |
| origin_https_port | (Required) - The HTTPS port the custom origin listens on | string | `443` | no |
| origin_keepalive_timeout | (Optional) The Custom KeepAlive timeout, in seconds. By default, AWS enforces a limit of 60. But you can request an increase. | string | `60` | no |
| origin_path | (Optional) - An optional element that causes CloudFront to request your content from a directory in your Amazon S3 bucket or your custom origin | string | `` | no |
| origin_protocol_policy | (Required) - The origin protocol policy to apply to your origin. One of http-only, https-only, or match-viewer | string | `match-viewer` | no |
| origin_read_timeout | (Optional) The Custom Read timeout, in seconds. By default, AWS enforces a limit of 60. But you can request an increase. | string | `60` | no |
| origin_ssl_protocols | (Required) - The SSL/TLS protocols that you want CloudFront to use when communicating with your origin over HTTPS | list | `<list>` | no |
| parent_zone_id | ID of the hosted zone to contain this record  (or specify `parent_zone_name`) | string | `` | no |
| parent_zone_name | Name of the hosted zone to contain this record (or specify `parent_zone_id`) | string | `` | no |
| price_class | Price class for this distribution: `PriceClass_All`, `PriceClass_200`, `PriceClass_100` | string | `PriceClass_100` | no |
| stage | Stage (e.g. `prod`, `dev`, `staging`) | string | - | yes |
| tags | Additional tags (e.g. `map('BusinessUnit','XYZ')`) | map | `<map>` | no |
| viewer_minimum_protocol_version | (Optional) The minimum version of the SSL protocol that you want CloudFront to use for HTTPS connections. | string | `TLSv1` | no |
| viewer_protocol_policy | allow-all, redirect-to-https | string | `redirect-to-https` | no |

## Outputs

| Name | Description |
|------|-------------|
| cf_aliases | Extra CNAMEs of AWS CloudFront |
| cf_arn | ID of AWS CloudFront distribution |
| cf_domain_name | Domain name corresponding to the distribution |
| cf_etag | Current version of the distribution's information |
| cf_hosted_zone_id | CloudFront Route 53 zone ID |
| cf_id | ID of AWS CloudFront distribution |
| cf_origin_access_identity | A shortcut to the full path for the origin access identity to use in CloudFront |
| cf_status | Current status of the distribution |




## Related Projects

Check out these related projects.

- [terraform-aws-cloudfront-s3-cdn](https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn) - Terraform module to easily provision CloudFront CDN backed by an S3 origin
- [terraform-aws-s3-log-storage](https://github.com/cloudposse/terraform-aws-s3-log-storage) - This module creates an S3 bucket suitable for receiving logs from other AWS services such as S3, CloudFront, and CloudTrail
- [terraform-aws-cloudtrail](https://github.com/cloudposse/terraform-aws-cloudtrail) - Terraform module to provision an AWS CloudTrail and an encrypted S3 bucket with versioning to store CloudTrail logs
- [terraform-aws-s3-website](https://github.com/cloudposse/terraform-aws-s3-website) - Terraform module to provision S3-backed Websites
- [terraform-root-modules/aws/docs](https://github.com/cloudposse/terraform-root-modules/tree/master/aws/docs) - Reference implementation combining `terraform-aws-s3-website` with `terraform-aws-cdn`



## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-cloudfront-cdn/issues), send us an [email][email] or join our [Slack Community][slack].

## Commerical Support

Work directly with our team of DevOps experts via email, slack, and video conferencing. 

We provide *commercial support* for all of our [Open Source][github] projects. As a *Dedicated Support* customer, you have access to our team of subject matter experts at a fraction of the cost of a fulltime engineer. 

[![E-Mail](https://img.shields.io/badge/email-hello@cloudposse.com-blue.svg)](mailto:hello@cloudposse.com)

- **Questions.** We'll use a Shared Slack channel between your team and ours.
- **Troubleshooting.** We'll help you triage why things aren't working.
- **Code Reviews.** We'll review your Pull Requests and provide constructive feedback.
- **Bug Fixes.** We'll rapidly work to fix any bugs in our projects.
- **Build New Terraform Modules.** We'll develop original modules to provision infrastructure.
- **Cloud Architecture.** We'll assist with your cloud strategy and design.
- **Implementation.** We'll provide hands on support to implement our reference architectures. 


## Community Forum

Get access to our [Open Source Community Forum][slack] on Slack. It's **FREE** to join for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build *sweet* infrastructure.

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-cloudfront-cdn/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://github.com/orgs/cloudposse/projects/3) with our other projects, we would love to hear from you! Shoot us an [email](mailto:hello@cloudposse.com).

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2018 [Cloud Posse, LLC](https://cloudposse.com)



## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.


## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know at <hello@cloudposse.com>

[![Cloud Posse](https://cloudposse.com/logo-300x69.svg)](https://cloudposse.com)

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We love [Open Source Software](https://github.com/cloudposse/)!

We offer paid support on all of our projects.  

Check out [our other projects][github], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.

  [docs]: https://docs.cloudposse.com/
  [website]: https://cloudposse.com/
  [github]: https://github.com/cloudposse/
  [jobs]: https://cloudposse.com/jobs/
  [hire]: https://cloudposse.com/contact/
  [slack]: https://slack.cloudposse.com/
  [linkedin]: https://www.linkedin.com/company/cloudposse
  [twitter]: https://twitter.com/cloudposse/
  [email]: mailto:hello@cloudposse.com


### Contributors

|  [![Erik Osterman][osterman_avatar]][osterman_homepage]<br/>[Erik Osterman][osterman_homepage] | [![Igor Rodionov][goruha_avatar]][goruha_homepage]<br/>[Igor Rodionov][goruha_homepage] | [![Andriy Knysh][aknysh_avatar]][aknysh_homepage]<br/>[Andriy Knysh][aknysh_homepage] | [![Justin Burnham][jburnham_avatar]][jburnham_homepage]<br/>[Justin Burnham][jburnham_homepage] |
|---|---|---|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: https://github.com/osterman.png?size=150
  [goruha_homepage]: https://github.com/goruha
  [goruha_avatar]: https://github.com/goruha.png?size=150
  [aknysh_homepage]: https://github.com/aknysh
  [aknysh_avatar]: https://github.com/aknysh.png?size=150
  [jburnham_homepage]: https://github.com/jburnham
  [jburnham_avatar]: https://github.com/jburnham.png?size=150


