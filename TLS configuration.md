TLS configuration is required for server installation, it's not relevant for purely desktop usage.

First thing to do is to get a TLS certificate. You have two options:

* Recommended - get TLS certificate signed by root certificate authority. For personal usage, best choice is [Let's encrypt](https://letsencrypt.org). It's free, automated and easy.
* generate your own self signed certificate. You will have extra trouble with importing the certificate into all machines from which you connect to the server installation so this is not recommended.

## Modifying config.ini

Now that you have your certificate, we need to modify `config.ini` in the [[data directory]] so that Trilium will use it:

```
[Network]
port=8080
# true for TLS/SSL/HTTPS (secure), false for HTTP (unsecure).
https=true
# path to certificate (run "bash bin/generate-cert.sh" to generate self-signed certificate). Relevant only if https=true
certPath=/[username]/.acme.sh/[hostname]/fullchain.cer
keyPath=/[username]/.acme.sh/[hostname]/example.com.key
``` 

Above is only example of how this is setup on my environment when I generated the certificate using Let's encrypt acme utility. Your paths may be completely different.

After you set this up, you may restart trilium and now visit the hostname with "https".