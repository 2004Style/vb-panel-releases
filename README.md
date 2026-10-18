# VB Panel · Releases

Instalador, paquetes compilados y checksums firmados de **VB Panel**: panel de control para desplegar servicios en un VPS con systemd, Caddy y Podman.

> Este repositorio solo contiene releases. El código fuente vive en un repositorio privado.

## Instalar

En un VPS con amd64 o arm64, como usuario con `sudo`:

```bash
curl -fsSL https://github.com/2004Style/vb-panel-releases/releases/latest/download/deploy.sh | bash
```

El asistente te guía por seis secciones: entorno, copias de seguridad, dominio, dependencias, GitHub (opcional) y administrador. Nada cambia hasta que confirmes el resumen.

## Versión fija

Para reproducir una instalación o volver a una versión anterior:

```bash
curl -fsSL https://github.com/2004Style/vb-panel-releases/releases/download/v0.1.1/deploy.sh | bash -s -- --version v0.1.1
```

Las versiones están en la sección [Releases](https://github.com/2004Style/vb-panel-releases/releases).

## Qué verifica el instalador

Antes de elevar privilegios y antes de extraer nada, el instalador comprueba la firma de `SHA256SUMS` con la clave pública de este repositorio, y después el checksum de cada archivo.

Huella de la clave de firma:

```
SHA256:UTIlW/Etugj1MXnXgO6+0B3d0LDuz03mgei06VU6kMk
```

Puedes comprobarla tú mismo: guarda la clave publicada en las notas de la release en un archivo y ejecuta `ssh-keygen -lf <archivo>`.

## Archivos de cada release

| Archivo | Contenido |
| --- | --- |
| `deploy.sh` | Instalador |
| `vb-panel_<versión>_linux_amd64.tar.gz` | Backend compilado (`api`, `migrate`, `bootstrap-admin`) |
| `vb-panel_<versión>_linux_arm64.tar.gz` | Backend compilado para arm64 |
| `vb-panel_<versión>_client.tar.gz` | Cliente web ya compilado |
| `SHA256SUMS` | Checksums de todos los archivos |
| `SHA256SUMS.sig` | Firma ed25519 de `SHA256SUMS` |

## Requisitos

- Ubuntu 22.04 o posterior, Debian 12 o posterior, Fedora 42 o posterior, o RHEL/Rocky/AlmaLinux/CentOS Stream 9 o posterior.
- amd64 o arm64.
- Usuario con permisos de `sudo`.
- Un dominio apuntando al VPS para el panel y la API.
