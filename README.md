# KodiRepo — JulioDev

Repositorio de addons para Kodi. Permite instalar y actualizar addons directamente desde Kodi sin necesidad de copiar archivos manualmente.

---

## Estructura del repositorio

```
KodiRepo/
├── addons.xml                        → Lista de addons disponibles
├── addons.xml.md5.txt                → Checksum para verificación
├── repository.JulioDev-1.0.0.zip    → ZIP para instalar el repositorio en Kodi
└── zips/
    └── script.multi-user.management/
        └── script.multi-user.management-1.0.0.zip
```

---

## Instalación en Kodi

### Paso 1 — Añadir la fuente

1. Kodi → **Ajustes** → **Administrador de archivos**
2. **Añadir fuente**
3. Introduce esta URL:
   ```
   https://juliodev00.github.io/KodiRepo
   ```
4. Ponle el nombre `JulioDev` y pulsa **Aceptar**

### Paso 2 — Instalar el repositorio

1. Kodi → **Complementos** → icono de caja (instalar desde ZIP)
2. Selecciona la fuente `JulioDev`
3. Selecciona `repository.JulioDev-1.0.0.zip`
4. Espera el mensaje **"Complemento instalado"**

### Paso 3 — Instalar el addon

1. Kodi → **Complementos** → **Instalar desde repositorio**
2. Selecciona **JulioDev Repository**
3. Busca **Multi User Management** e instala

---

## Addons disponibles

| Addon | ID | Versión | Descripción |
|---|---|---|---|
| Multi User Management | `script.multi-user.management` | 1.0.0 | Gestión multiusuario con tokens de Trakt.tv |

---

## Cómo actualizar un addon

Cuando hagas cambios en el addon y quieras publicar una nueva versión:

### 1. Actualiza la versión en `addon.xml` del addon

```xml
<addon id="script.multi-user.management" version="1.0.1" ...>
```

### 2. Genera el nuevo ZIP del addon

- Mete la carpeta `script.multi-user.management` en un ZIP
- Nómbralo con la nueva versión:
  ```
  script.multi-user.management-1.0.1.zip
  ```
- Súbelo a `zips/script.multi-user.management/`
- El ZIP anterior puede quedarse (Kodi gestiona versiones)

### 3. Actualiza `addons.xml`

Cambia la versión en el `addons.xml` de la raíz:

```xml
<addon id="script.multi-user.management" version="1.0.1" ...>
```

### 4. Regenera el MD5

Abre PowerShell en la raíz del repo y ejecuta:

```powershell
Get-FileHash addons.xml -Algorithm MD5 | Select-Object -ExpandProperty Hash | Out-File addons.xml.md5.txt -Encoding ASCII -NoNewline
```

### 5. Sube los cambios a GitHub

Archivos a actualizar en GitHub:
- `addons.xml` (versión actualizada)
- `addons.xml.md5.txt` (nuevo hash)
- El nuevo ZIP en `zips/script.multi-user.management/`

Kodi detectará la actualización automáticamente.

---

## Cómo añadir un nuevo addon

### 1. Prepara el addon

Asegúrate de que tiene esta estructura:

```
mi-nuevo-addon/
├── addon.xml
├── default.py
├── icon.png
└── resources/
```

### 2. Crea el ZIP

- Comprime la carpeta entera
- Nómbralo: `mi-nuevo-addon-1.0.0.zip`
- Súbelo a: `zips/mi-nuevo-addon/`

### 3. Añádelo al `addons.xml`

Agrega una entrada dentro de `<addons>`:

```xml
<addon id="mi-nuevo-addon" name="Nombre" version="1.0.0" provider-name="JulioDev">
  <requires>
    <import addon="xbmc.python" version="3.0.0"/>
  </requires>
  <extension point="xbmc.python.script" library="default.py">
    <provides>executable</provides>
  </extension>
  <extension point="xbmc.addon.metadata">
    <summary>Descripción del addon</summary>
    <platform>all</platform>
  </extension>
</addon>
```

### 4. Regenera el MD5 y sube los cambios

Mismo proceso que al actualizar (pasos 4 y 5 de arriba).

---

## Instalar el repoitorio
```
    https://juliodev00.github.io/KodiRepo
```
## Notas técnicas

- El repositorio usa **GitHub Pages** para servir los archivos
- Compatible con **Kodi 19 (Matrix), 20 (Nexus) y 21 (Omega)**
- El archivo `addons.xml.md5.txt` debe estar sincronizado con `addons.xml` — si no coinciden Kodi no conecta
