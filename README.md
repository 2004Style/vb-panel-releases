# VB Panel · Releases

Instalador, paquetes compilados y checksums firmados de **VB Panel**: panel de control para desplegar servicios en un VPS con systemd, Caddy y Podman.

> Este repositorio solo contiene releases. El código fuente vive en un repositorio privado.

## Instalar

En un VPS con amd64 o arm64, como usuario con `sudo`:

```bash
curl -fsSL https://github.com/2004Style/vb-panel-releases/releases/latest/download/deploy.sh | bash
```

El asistente te guía por seis secciones: entorno, copias de seguridad, dominio, dependencias, GitHub (opcional) y administrador. Nada cambia hasta que confirmes el resumen.

Para instalar una versión concreta:

```bash
curl -fsSL https://github.com/2004Style/vb-panel-releases/releases/download/v0.1.7/deploy.sh | bash -s -- --version v0.1.7
```

Las versiones están en la sección [Releases](https://github.com/2004Style/vb-panel-releases/releases).

## Actualizar

Actualiza una instalación que ya hiciste con este instalador. Antes de cambiar nada guarda la configuración y un volcado de la base del panel en `/var/backups/vb-panel-install/`.

Descarga el instalador de la versión a la que quieres actualizar y ejecútalo:

```bash
# Última versión publicada
curl -fsSL https://github.com/2004Style/vb-panel-releases/releases/latest/download/deploy.sh -o deploy.sh
sudo bash deploy.sh --upgrade

# Versión concreta
curl -fsSL https://github.com/2004Style/vb-panel-releases/releases/download/v0.1.7/deploy.sh -o deploy.sh
sudo bash deploy.sh --upgrade --version v0.1.7
```

La actualización no hace preguntas y no reescribe `/etc/vb-panel/api.env`. Conserva las releases anteriores, pero no hay migración hacia atrás automática: volver a una versión anterior contra un esquema más nuevo puede ser rechazado.

## Desinstalar

Elimina VB Panel de ese servidor. Solo retira lo que creó el instalador; el resto del VPS no se toca. Requiere v0.1.7 o posterior.

```bash
curl -fsSL https://github.com/2004Style/vb-panel-releases/releases/latest/download/deploy.sh -o deploy.sh
sudo bash deploy.sh --uninstall
```

Antes de borrar muestra el plan y crea un respaldo obligatorio en `/var/backups/vb-panel-uninstall/<fecha UTC>/`, incluido el volcado de la base del panel. Si el respaldo falla, no se elimina nada. Para confirmar, escribe `ELIMINAR` en mayúsculas; cualquier otro texto cancela.

Opciones peligrosas, solo si sabes lo que haces:

- `--yes-eliminar-todo`: confirma sin escribir `ELIMINAR`. El respaldo sigue siendo obligatorio.
- `--no-db-backup`: elimina aunque no se pueda volcar PostgreSQL. Pierdes esos datos.

## Verificación

Antes de elevar privilegios y antes de extraer nada, el instalador comprueba la firma de `SHA256SUMS` con la clave pública de este repositorio, y después el checksum de cada archivo.

Huella de la clave de firma:

```
SHA256:UTIlW/Etugj1MXnXgO6+0B3d0LDuz03mgei06VU6kMk
```

Puedes comprobarla tú mismo: guarda la clave publicada en las notas de la release en un archivo y ejecuta `ssh-keygen -lf <archivo>`.

## Archivos de cada release

| Archivo | Contenido |
| --- | --- |
| `deploy.sh` | Instalador, actualización y desinstalación |
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
