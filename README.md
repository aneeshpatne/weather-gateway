# Weather Gateway (NGINX Reverse Proxy)

This repo contains the NGINX configuration for a simple **weather gateway**.

It exposes multiple internal services (running on different IPs/ports on the LAN) under **one server / one domain**.

## 🌐 Goal

You currently access your services like this:

- `http://192.168.1.50/sensors_v2`
- `http://192.168.1.100:8008/pm25/avg`
- `http://192.168.1.100:8008/alerts/remark`

With this NGINX config, you will be able to hit them via a single gateway server:

- `http://<GATEWAY_IP>/sensors`
- `http://<GATEWAY_IP>/pm25`
- `http://<GATEWAY_IP>/alerts`

Or, if you have a hostname (e.g. `weather.local`):

- `http://weather.local/sensors`
- `http://weather.local/pm25`
- `http://weather.local/alerts`

---

## 📁 Repo Structure

```text
weather-gateway/
├── nginx/
│   └── weather.conf
└── README.md
```

---

## 🚀 Setup

### 1. Install NGINX

```bash
# Debian/Ubuntu
sudo apt update && sudo apt install nginx -y

# CentOS/RHEL
sudo yum install nginx -y
```

### 2. Copy the Configuration

```bash
sudo cp nginx/weather.conf /etc/nginx/sites-available/weather.conf
sudo ln -s /etc/nginx/sites-available/weather.conf /etc/nginx/sites-enabled/
```

### 3. Test & Reload NGINX

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## ⚙️ Configuration

Edit `nginx/weather.conf` to customize:

- **`server_name`** – Set your domain or IP (e.g., `weather.local` or `_` for any)
- **`proxy_pass`** – Update the internal service IPs/ports as needed

### Example Location Blocks

```nginx
location /sensors {
    proxy_pass http://192.168.1.50/sensors_v2;
}

location /pm25 {
    proxy_pass http://192.168.1.100:8008/pm25/avg;
}

location /alerts {
    proxy_pass http://192.168.1.100:8008/alerts/remark;
}
```

---

## 🔧 Optional: Set Up Local DNS

To access via `http://weather.local`, add an entry to your `/etc/hosts` file on client machines:

```bash
<GATEWAY_IP>    weather.local
```

---

## 📝 License

MIT
