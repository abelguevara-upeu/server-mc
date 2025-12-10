# Servidor Minecraft (Docker + GitOps)

Este repositorio contiene la configuración completa de mi servidor de Minecraft.
Incluye sistema de respaldo automático y gestión de mods vía Git.

## 🚀 Instalación Rápida en VPS Nuevo

Para instalar este servidor en un VPS limpio (Ubuntu/Debian), simplemente copia y pega este bloque:

```bash
# ==========================================
# SCRIPT DE INSTALACIÓN (COPIAR Y PEGAR)
# ==========================================
cat > install.sh << 'EOF'
#!/bin/bash
echo ">>> PREPARANDO SISTEMA..."
apt update && apt install -y docker.io docker-compose git

echo ">>> DATOS DE GITHUB..."
echo "Usuario:" && read USER
echo "Token (PAT):" && read -s TOKEN
echo "Repo:" && read REPO

echo ">>> DESCARGANDO..."
rm -rf /home/minecraft-docker
git clone "https://$USER:$TOKEN@github.com/$USER/$REPO.git" /home/minecraft-docker

echo ">>> INICIANDO..."
cd /home/minecraft-docker
# Intentar usar el comando moderno (v2), si falla usar el antiguo (v1)
docker compose up -d || docker-compose up -d
echo "¡LISTO!"
EOF
chmod +x install.sh && ./install.sh
```

## 🛠️ Cómo gestionar el servidor

### Agregar Mods
1. Pon el `.jar` en la carpeta `data/mods` de tu PC.
2. `git add .` -> `git commit` -> `git push`.
3. En el VPS: `cd /home/minecraft-docker && git pull && docker-compose restart mc`.

### Ver la consola
```bash
docker logs -f mc-server
```

### Forzar Respaldo Manual
El respaldo es automático cada 24h, pero si quieres forzarlo:
```bash
docker restart mc-backup
```
