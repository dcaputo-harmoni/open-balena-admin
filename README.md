# open-balena-admin
Open Balena Admin

Instructions for building using docker-compose on localhost (for production just replcae openbalena.local with your domain)

1. Download open-balena (git clone https://github.com/balena-io/open-balena.git)
2. Configure open-balena (run scripts/quickstart -U balena@openbalena.local -P balena)
3. Edit /etc/hosts or C:\Windows\System32\Drivers\etc\hosts to include:

127.0.0.1 api.openbalena.local
127.0.0.1 registry.openbalena.local
127.0.0.1 vpn.openbalena.local
127.0.0.1 s3.openbalena.local
127.0.0.1 tunnel.openbalena.local
127.0.0.1 admin.openbalena.local

(Note: if in production, set up the same hostnames above to point to your public IP)

4. Run open-balena (run scripts/compose up)
5. Download open-balena-admin (git clone https://github.com/dcaputo-harmoni/open-balena-admin.git)
6. Configure open-blena-admin (run scripts/quickstart -j [OPENBALENA_JWT_SECRET]) 
NOTE: you can find OPENBALENA_JWT_SECRET in your open-balena folder in the file config/activate
7. Run open-balena-admin (run scripts/compose up)

Now you are ready to navigate to admin.openbalena.local:8080 and log in using the credentials from step 2 above.