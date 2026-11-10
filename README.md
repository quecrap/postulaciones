# 🚀 Tarjeta Digital & vCard - Pablo Campos Moreno

> **"Ingeniero multidisciplinario de cuello gris: del diseño al desliz."**

Sitio web estático ultraligero y minimalista diseñado para servir como tarjeta de presentación digital interactiva y vCard descargable con código QR de alta resolución. Preparado para despliegue directo en **Azure Static Web Apps (Free Tier)**.

---

## 📁 Estructura del Proyecto

```text
postulaciones/
├── .github/
│   └── workflows/
│       └── azure-static-web-apps.yml   # Workflow automatizado para CI/CD con GitHub Actions
├── .gitignore                          # Exclusiones estándar para Git
├── contact.vcf                         # Archivo vCard estándar (RFC 2426 / 3.0)
├── index.html                          # Página principal (Dark dev/terminal theme, QR dinámico)
├── staticwebapp.config.json            # Configuración de rutas, MIME types y headers para Azure
└── README.md                           # Instrucciones de uso y despliegue
```

---

## 🛠️ Pruebas en Local

No necesitas instalar frameworks ni herramientas complejas:

### Opción 1: Abrir directo en el navegador
Haz doble clic sobre [index.html](file:///c:/Users/PEPITO/Downloads/postulaciones/index.html) o arrástralo a Chrome/Edge.

### Opción 2: Servidor local ligero (Recomendado)
Abre una terminal en esta carpeta y ejecuta cualquiera de los siguientes comandos:

**Con Python:**
```bash
python -m http.server 8000
```
Luego entra en tu navegador a `http://localhost:8000`.

**Con Node / npx:**
```bash
npx serve .
```

---

## ✏️ Personalización de Datos de Contacto

1. **Datos de la vCard (`contact.vcf`)**:
   Abre [contact.vcf](file:///c:/Users/PEPITO/Downloads/postulaciones/contact.vcf) y actualiza:
   - `TEL`: Tu número telefónico real (ejemplo: `+52 1 55 1234 5678`).
   - `EMAIL`: Tu correo electrónico real.
   - `URL`: Tu enlace de perfil de LinkedIn o portafolio.

2. **URL Definitiva de Azure en `index.html`**:
   En [index.html](file:///c:/Users/PEPITO/Downloads/postulaciones/index.html), línea 264:
   ```javascript
   const AZURE_DEPLOY_URL = "https://tu-nombre-o-app.azurestaticapps.net";
   ```
   > **Nota de conveniencia:** El código ya detecta automáticamente si el sitio está ejecutándose en producción en Azure para generar el QR con la URL real actual. Esta constante sirve como respaldo cuando lo pruebas en local.

---

## ☁️ Despliegue en Microsoft Azure (Capa Gratuita / Free Tier)

Azure Static Web Apps ofrece en su **Free Tier**:
- Ancho de banda y hosting global SSL gratuito ilimitado para sitios personales.
- Soporte para dominios personalizados sin costo (incluyendo certificado SSL gestionado).
- CI/CD automático mediante GitHub Actions.

### Método 1: Desde Azure Portal + GitHub (Recomendado, 2 minutos)

1. Sube esta carpeta `postulaciones` a un nuevo repositorio en GitHub:
   ```bash
   git init
   git add .
   git commit -m "feat: tarjeta de presentacion vcard inicial"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/TU_REPOSITORIO.git
   git push -u origin main
   ```

2. Entra al [Portal de Microsoft Azure](https://portal.azure.com/).
3. Busca **Static Web Apps** y haz clic en **Crear**.
4. Completa la configuración:
   - **Suscripción**: Selecciona tu suscripción.
   - **Grupo de recursos**: Elige uno existente o crea uno (ej. `rg-postulaciones`).
   - **Nombre**: `pablo-campos-vcard` (o el que prefieras).
   - **Plan type**: **Free** (Gratuito).
   - **Región**: `Central US` o `West US 2`.
   - **Origen de la implementación**: Selecciona **GitHub** e inicia sesión.
   - **Organización / Repositorio / Rama**: Elige tu repositorio y la rama `main`.
   - **Valores preestablecidos de compilación**: Elige `Personalizado` (Custom):
     - **Ubicación de la aplicación (App location)**: `/`
     - **Ubicación de la API (Api location)**: *(dejar vacío)*
     - **Ubicación de los artefactos de salida (Output location)**: *(dejar vacío)*
5. Haz clic en **Revisar y crear** -> **Crear**.
6. Azure configurará el secreto y el archivo en `.github/workflows/`, desplegando el sitio en menos de 1 minuto. Te entregará una URL del estilo `https://gentle-ocean-xxx.azurestaticapps.net`.

---

## 🔒 Buenas Prácticas de Infraestructura Incluidas

- **MIME Types para vCard**: En `staticwebapp.config.json` se mapea `.vcf` a `text/vcard` con cabecera `Content-Disposition: attachment` para garantizar que en iPhone (iOS Contactos) y Android se descargue e importe con un solo toque.
- **Seguridad HTTP**: Headers de seguridad preconfigurados (`X-Content-Type-Options: nosniff`, `X-Frame-Options: SAMEORIGIN`, `Referrer-Policy`).
- **Nivel H de Corrección de Error en QR (30%)**: Permite que el código QR sea legible incluso si se imprime en papel, se ensucia o se escanea bajo condiciones de baja iluminación o ángulos complejos.

