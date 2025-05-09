# 🐳 Docker Webserver met NGINX

Deze repository bevat een eenvoudige handleiding om Docker te installeren op Ubuntu en een statische website te hosten met behulp van NGINX in een Docker-container.

## 📦 Inhoud

- Installatie van Docker op Ubuntu
- Testen van Docker-installatie
- Host je eigen HTML-pagina via een NGINX-container
- Beveiligingsnotities m.b.t. Docker-gebruikersgroep

## 🔧 Installatie

Volg de gedetailleerde stappen in [`docker_handleiding.md`](./docker_handleiding.md) of gebruik de samenvatting hieronder.

### 1. Vereisten installeren

```bash
sudo apt update
sudo apt install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

### 2. Docker installeren

```bash
# GPG-sleutel toevoegen
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Repository toevoegen
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Docker installeren
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

### 3. Toegang zonder `sudo` (optioneel)

```bash
sudo usermod -aG docker $USER
newgrp docker
```

## 🚀 Eenvoudige Webserver

### Start een standaard nginx-webserver

```bash
docker run -d -p 8080:80 --name webserver nginx
```

Bezoek in je browser: [http://localhost:8080](http://localhost:8080)

### Gebruik eigen HTML-bestanden

```bash
mkdir ~/mijn-website
echo "<h1>Hallo vanuit Docker!</h1>" > ~/mijn-website/index.html

docker run -d -p 8080:80 \
  -v ~/mijn-website:/usr/share/nginx/html:ro \
  --name webserver-custom \
  nginx
```

## ⚠️ Veiligheidswaarschuwing

Voeg alleen vertrouwde gebruikers toe aan de `docker`-groep. Deze groep geeft root-achtige machtigingen via de Docker daemon.

## 📄 Licentie

Vrij te gebruiken voor educatieve en persoonlijke projecten. Geen garantie.
