<div align="center">
  <img src="./assets/logo.svg" width="760" alt="CINÉ-GENESIS — estudio de cine con IA">
  <p>
    <img src="./assets/badges/status.svg" alt="Estado: activo">
    <img src="./assets/badges/version.svg" alt="Versión: 1.0.0">
    <img src="./assets/badges/license.svg" alt="Licencia: AGPL-3.0-or-later">
    <img src="./assets/badges/browser.svg" alt="Compatibilidad: navegadores modernos">
  </p>
  <p><a href="./README.md">Esperanto</a> · <a href="./README.fr.md">Français</a> · <a href="./README.en.md">English</a> · <strong>Español</strong></p>
</div>

# CINÉ-GENESIS

Aplicación web autónoma para crear cortometrajes con inteligencia artificial,
desde la idea inicial hasta las imágenes y los planos de vídeo.

## Funciones

| Función | Descripción |
|---|---|
| Creación guiada | Siete pasos estructurados desde el género hasta la película |
| Continuidad visual | Referencias compartidas entre personajes, escenas y planos |
| Generación de vídeo | Producción plano a plano con seguimiento automático |
| Recuperación | Guardado local automático después de una interrupción |
| Varias claves | Distribución de solicitudes entre varias claves API |
| Privacidad | Claves limitadas a la pestaña y exportaciones saneadas |
| Proyecto portátil | Exportación completa en formato JSON |

## API necesaria

CINÉ-GENESIS utiliza exclusivamente la API de
[Agnès AI](https://agnes-ai.com/). Las claves de OpenAI, Anthropic, Google
Gemini u otros proveedores no funcionan.

1. Abre el sitio oficial de Agnès AI.
2. Elige **Free API** o **Access API**.
3. Crea o copia una clave en el espacio para desarrollo.
4. Pégala en la primera pantalla de CINÉ-GENESIS.

Los planes, cuotas, precios y nombres de modelos pueden cambiar. Comprueba las
condiciones actuales de Agnès antes de una generación importante.

## Inicio rápido

Un solo comando clona y sirve la aplicación localmente:

```bash
git clone https://github.com/AnARCHIS12/Voanh-Agnes-film-maker.git && cd Voanh-Agnes-film-maker && python3 -m http.server 8080
```

Después abre `http://localhost:8080` en un navegador moderno.

## Flujo de creación

1. **Conexión** — introduce las claves de Agnès y elige el plan.
2. **Formato** — selecciona el género y la proporción de imagen.
3. **Sinopsis** — redacta libremente o solicita una revisión a la IA.
4. **Personajes** — crea perfiles, vestuario y retratos de referencia.
5. **Escenas** — define decorados, ambiente, diálogos e imágenes.
6. **Planos** — genera el desglose cinematográfico y los fotogramas iniciales.
7. **Vídeos** — genera, supervisa y reproduce la secuencia completa.

Cada resultado se puede editar antes de pasar al siguiente paso.

## Configuración técnica

| Uso | Valor |
|---|---|
| API principal | `https://apihub.agnes-ai.com/v1` |
| Seguimiento de vídeo | `https://apihub.agnes-ai.com/agnesapi` |
| Autenticación | `Authorization: Bearer TU_CLAVE` |
| Texto | `agnes-3.0-flash`, después `agnes-2.5-flash` |
| Imagen | `agnes-image-2.5-flash` |
| Vídeo gratuito | `agnes-video-v2.0` |
| Vídeo Token Plan | `agnes-video-2.5-flash` |

La aplicación utiliza `POST /v1/chat/completions`,
`POST /v1/images/generations`, `POST /v1/videos` y
`GET /agnesapi?video_id=...&model_name=...`.

## Arquitectura

La aplicación no tiene un backend propio. La interfaz, los estilos y la lógica
residen en `index.html`. Los proyectos se guardan en `IndexedDB`, con
`localStorage` como mecanismo alternativo.

```text
Proyecto
├── configuración, género y sinopsis
├── personajes
│   └── perfil e imagen de referencia
└── escenas
    ├── decorado, ambiente, diálogos e imagen
    └── planos
        └── acción, cámara, frase, imagen y vídeo
```

```text
.
├── assets/
│   ├── badges/          Insignias SVG locales
│   └── logo.svg         Identidad visual del proyecto
├── index.html           Aplicación completa
├── LICENSE              AGPL-3.0-or-later
├── README.md            Esperanto
├── README.fr.md         Francés
├── README.en.md         Inglés
└── README.es.md         Español
```

## Privacidad y seguridad

Los proyectos permanecen en el navegador, pero las funciones de IA no son
locales. Los textos, diálogos, descripciones, referencias visuales y fotos
confirmadas se envían a Agnès AI. Los vídeos pueden permanecer disponibles
mediante una URL remota.

- Las claves permanecen en la pestaña actual de forma predeterminada.
- La persistencia local es opcional y revocable.
- Las claves se eliminan de las exportaciones JSON.
- Se rechazan las URL no cifradas o claramente locales.
- Una política de seguridad de contenido limita las capacidades de la página.

No envíes datos personales, obras confidenciales o imágenes de terceras personas
sin los derechos adecuados y un consentimiento informado.

## Compatibilidad

| Entorno | Compatibilidad | Notas |
|---|---|---|
| Navegadores Chromium | Completa | Recomendados para vídeo y síntesis de voz |
| Firefox | Completa | Las voces disponibles dependen del sistema |
| Safari | Versión reciente | Comprueba las restricciones de reproducción automática |
| Navegadores móviles | Interfaz adaptable | Mantén la pestaña abierta durante generaciones largas |
| Sin conexión | Parcial | La edición funciona; la generación necesita Agnès |

## Contribuir

1. Crea un fork y una rama específica.
2. Mantén el proyecto sin CDN externos, telemetría ni secretos.
3. No añadas emojis a la interfaz, documentación o commits.
4. Verifica la sintaxis JavaScript y el diseño adaptable.
5. Usa un commit convencional como `fix: protect exported API keys`.
6. Abre una solicitud de fusión con los controles realizados.

## Licencia

Este proyecto es software libre distribuido bajo la
[GNU Affero General Public License v3.0 o posterior](./LICENSE). Las versiones
modificadas accesibles por red también deben ofrecer su código fuente
correspondiente según los términos de la licencia.
