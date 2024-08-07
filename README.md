
## What is this?
This project is symbiosis between [WireHole](https://github.com/IAmStoxe) and [Wireguard Easy](https://github.com/wg-easy/wg-easy). 

**WireHole Easy** is a docker-compose project that combines WireGuard, PiHole, and Unbound to create a full or split-tunnel VPN that is easy to deploy and manage. This setup allows for a VPN with ad-blocking via PiHole and enhanced DNS privacy and caching through Unbound.

In addition, you have WEB UI for creating configs for you clients. Open http://<you_ip_address>:1122

---

### Quickstart

To begin using WireHole, clone the repository and start the containers:

```bash
# if you don't have docker
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $(whoami)
sudo reboot

# You need to change the password below
WEB_UI_PASSWORD=<CHANGE_THIS_PASSWORD>

# Clone the WireHole repository from GitHub
git clone https://github.com/insigmo/wirehole_easy.git

# Change directory to the cloned repository
cd wirehole_easy/

# Update the .env file with your configuration
cp .env.template .env

# Replace the public IP placeholder in the docker-compose.yml
sed -i "s/PUBLIC_IP_ADDRESS/$(curl -s ifconfig.me)/g" .env
sed -i "s/WEB_UI_PASSWORD/$WEB_UI_PASSWORD/g" .env
# Start the Docker containers
docker compose up -d
```

## Environment Configuration Details

The `.env` file contains a series of environment variables that are essential for configuring the WireHole services within the Docker containers. Here is a detailed explanation of each variable:

### General Settings

- `TIMEZONE`: Sets the timezone for all services. It is important for logging and time-based operations. Example: `America/Los_Angeles`.

### User / Group Identifiers

- `PUID` and `PGID`: These ensure that the services have the correct permissions to access and modify files on the host system. They should match the user ID and group ID of the host user.

### Network Settings

- `UNBOUND_IPV4_ADDRESS`: The static IP address assigned to Unbound, ensuring it is reachable by Pi-hole.
- `PIHOLE_IPV4_ADDRESS`: The static IP address assigned to Pi-hole, allowing it to serve DNS requests for the network.
- `WG_PORT`: The port on which the WireGuard server will listen for connections.

### WireGuard-UI Settings
- `WG_HOST`: You public ip address
- `PASSWORD`: A web ui password
- `PORT`: A web ui port

### Pi-hole Settings
- `WEBPASSWORD`: The password for accessing the Pi-hole web interface. It should be set to a secure value to prevent unauthorized access.
- `PIHOLE_DNS`: The IP address of the Unbound server used by Pi-hole to resolve DNS queries.

Remember to replace any default or placeholder values with secure, unique values before deploying your services.

---

### Recommended Configuration / Split Tunnel

For a split-tunnel VPN, configure your WireGuard client `AllowedIps` to `10.2.0.0/24`, which will route only the web panel and DNS traffic through the VPN.

---

### Access PiHole

Connect to WireGuard and access the Pi-hole admin panel at `http://10.2.0.100/admin`. The login password is the one set as `WEBPASSWORD` in your `.env` file.

---

### Dynamic DNS (DDNS)

Configure DDNS by setting `WG_HOST` in your `.env` file to your DDNS URL.

```yaml
wireguard:
  environment:
    - WG_HOST=my.ddns.net
```

---

### Configuring / Parameters

Explain all the environment variables from your `.env` file here. (Refer to the previous section where we provided a table of explanations for each variable.)

---

### Additional Settings and Considerations

Discuss any additional settings such as Docker secrets, umask settings, user/group identifiers, adding clients, modifying DNS providers, and networking considerations. Make sure to update any instructions to match the current setup.
