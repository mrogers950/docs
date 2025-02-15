---
slug: enabling-https-using-certbot-with-nginx-on-debian
title: "Use Certbot to Enable HTTPS with NGINX on Debian"
description: "Learn how to install and use Certbot with NGINX on Debian 10 and 9, which automates the process adding TLS/SSL to your websites."
authors: ["Linode"]
contributors: ["Linode"]
published: 2021-07-01
keywords: ["Debian", "Certbot", "TLS", "SSL", "HTTPS", "Encryption", "NGINX"]
tags: ["debian", "ssl", "nginx"]
license: '[CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0)'
aliases: ['/guides/how-to-install-certbot-on-debian-10/']
relations:
    platform:
        key: how-to-use-certbot-with-nginx
        keywords:
            - distribution: Debian
---

This guide provides instructions on using the open source [Certbot](https://certbot.eff.org/) utility with the NGINX web server on Debian 10 and 9. Certbot dramatically reduces the effort (and cost) of securing your websites with HTTPS. It works directly with the free [Let's Encrypt](https://letsencrypt.org/) certificate authority to request (or renew) a certificate, prove ownership of the domain, and install the certificate on NGINX (or other web servers).

**Supported distributions:** Debian 10 (Buster) and Debian 9 (Stretch). Debian 8 (Jessie) is no longer supported by Certbot.

## Before You Begin

Before continuing with this guide, you need a website accessible over HTTP using your desired domain name. Breaking this down further, the following components are required:

1.  **A server running on Debian 10 or 9** with credentials to a standard user account (belonging to the `sudo` group) and the ability to access the server through[SSH](/docs/guides/connect-to-server-over-ssh/) or [Lish](/docs/products/compute/compute-instances/guides/lish/). [Creating a Compute Instance](/docs/products/compute/compute-instances/guides/create/) and [Setting Up and Securing a Compute Instance](/docs/products/compute/compute-instances/guides/set-up-and-secure/) guides for information on deploying and configuring a Linode Compute Instance.

2.  **A registered domain name with DNS records pointing to the IPv4 (and optionally IPv6) address of your server.** A domain can be obtained through any registrar and can utilize any DNS service, such as Linode's [DNS Manager](/docs/products/networking/dns-manager/). Review the [DNS Records: An Introduction](/docs/guides/dns-overview/) guide for more information on configuring DNS.

3.  **The NGINX web server software installed on your server and configured for your domain.** You can review the [How to Install NGINX on Debian 10](/docs/guides/how-to-install-nginx-debian-10/) guide for information on installing and configuring NGINX.

{{< note >}}
This guide is written for a non-root user. Commands that require elevated privileges are prefixed with `sudo`. If you are not familiar with the `sudo` command, see the [Users and Groups](/docs/guides/linux-users-and-groups/) guide.
{{< /note >}}

{{< content "understanding-https-tls-certbot-shortguide" >}}

{{< content "configuring-ufw-for-web-traffic-shortguide" >}}

{{< content "installing-snapd-certbot-apt-shortguide" >}}

{{< content "requesting-certificate-nginx-certbot-shortguide" >}}

{{< content "testing-https-certbot-shortguide" >}}

{{< content "renewing-certificate-certbot-shortguide" >}}

{{< content "deleting-certificate-certbot-shortguide" >}}

{{< content "learn-more-certbot-shortguide" >}}