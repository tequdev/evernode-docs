# SSL

During the [installation](../../hosts/evernode-host), if you opted in to use [Let's Encyrypt](https://letsencrypt.org/) SSL certificates, Sashimono would integrate with Let's Encrypt [cerbot](https://certbot.eff.org/pages/about) software to acquire and renew SSL certificates for your host's domain. This is fully automated and you do not have to do anything. Following explanation is for informational purposes only.

1. Upon installation, new SSL certificates issued from Let's Encrypt are acquired for your domain using certbot. Certbot places them in `/etc/letsencrypt/live/<your domain>` directory.
2. HotPocket smart contract instances uses these SSL ceriticates on its websocket channels so they could communicate with wide range of clients including web browsers.
2. Let's Encrypt SSL certificates expire every 3 months. Certificate renewals are automatically handled.
3. When SSL certificates are auto-renewed, all existing contracts will be updated with new SSL certificates. This will cause all existing contract instances to be stopped and started for a brief period (few seconds). This is performed with a script placed in `/etc/letsencrypt/renewal-hooks/deploy/` directory.