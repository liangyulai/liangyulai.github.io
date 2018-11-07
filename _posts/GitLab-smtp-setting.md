---
title: 'GitLab: smtp setting'
date: 2016-07-21 14:51:04
tags:
  - GitLab
  - SMTP
---


[GitLab SMTP settings](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/doc/settings/smtp.md)

If you would rather send application email via an SMTP server instead of via Sendmail, add the following configuration information to /etc/gitlab/gitlab.rb and run gitlab-ctl reconfigure.

#### Outlook

```
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp-mail.outlook.com"
gitlab_rails['smtp_port'] = 587
gitlab_rails['smtp_user_name'] = "odc-gitlab@outlook.com"
gitlab_rails['smtp_password'] = "******"
gitlab_rails['smtp_domain'] = "ODC-Gitlab"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
# gitlab_rails['smtp_tls'] = true
gitlab_rails['smtp_openssl_verify_mode'] = 'peer'
```
