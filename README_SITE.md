# Site del alumnado *· Zensical

El punto de entrada es **Document_de_treball.md**. El site presenta su contenido como portada, con el mapa semanal y los enlaces a los materiales. La navegación mujer acceso a descargas, materiales prácticos, píldoras breves y píldoras ampliadas.

## Editar y previsualitzar

Desde la carpeta `ALUMNAT`, crea una vez el entorno e instala Zensical:

```bash
python3 -m venv .venv-site
source .venv-site/bin/activate
python -m pip install -r requirements-site.txt
```

Para construir y abrir un servidor local:

```bash
python site_projecte.py serve
```

Abre `http://127.0.0.1:8000/`. Para pararlo, pulsa Ctrl+C. Este servidor muestra la versión construida y comprobada; después de modificar un material, páralo y vuelve a ejecutar el mando porque se reconstruya. No es una previsualización con actualización automática.

Para generar solo los ficheros publicables:

```bash
python site_projecte.py build
```

El resultado está en `site/`. El proceso construye con Zensical en modo estricto y comprueba los enlaces locales, las àncores y las descargas. No requiere las bibliotecas de modelado ni ejecuta los notebooks o los ejemplos de las píldoras.

## Una única fuente de contenido

Edita el documento de trabajo y los materiales en las carpetas actuales. **No editas `web_docs/` ni `site/`**: se regeneran. `web_docs/index.md` es una copia automática del documento de trabajo con accesos a las descargas y las píldoras ampliadas.

El proceso incorpora solo `DOCUMENT_DE_TREBALL.md`, `MATERIALS_PRACTICS`, `PINDOLES` y `PINDOLES_AMPLIADES`, con una lista explícita de formatos admitidos. Los resultados de ejecuciones, los entornos y los ficheros de configuración de la biblioteca antigua no se publican. El profesorado queda fuera del site.

La configuración de navegación, idioma, URL y presentación está en `zensical.toml`. Los estilos adicionales están en `web_assets/projecte.css`. La configuración MkDocs de las píldoras ampliadas se conserva para el uso independiente, pero el site conjunto se construye con Zensical.

## Descargas

Los enlaces Markdown locales a `.csv`, `.ipynb` y `.py` reciben automáticamente el atributo HTML `download` en la copia web. Así descargan el fichero original desde el mismo dominio de GitHub Pages. No apuntan a `github.com/.../blob/` ni dependen de Javascript. La preferencia de carpeta o el diálogo de desament depende del navegador.

La página **Descargas** enumera todos los ficheros de estos tipos y ofrece un ZIP con los materiales prácticos. El ZIP conserva lo estructura `MATERIALS_PRACTICS/notebooks` y `MATERIALS_PRACTICS/dades`, necesaria para las rutas relativas de los notebooks.

Se comprueba que cada fichero publicado es idéntico al de la copia de construcción. Si añades enlaces con una sintaxis diferente de los enlaces Markdown habituales, la comprobación detectará que falta el atributo de descarga e impedirá publicar una construcción incompleta.

## GitHub Pages

El remoto detectado al inicio de la preparación era `https://github.com/IesElJust/iabd-26-27.git`. La configuración prevé la dirección:

`https://ieseljust.github.io/iabd-26-27/`

La carpeta `ALUMNAT` es la raíz del repositorio. El workflow está en `.github/workflows/pages.yml` dentro de esta carpeta; `DOCUMENT_DE_TREBALL.md`, `zensical.toml` y `site_projecte.py` quedan al primer nivel. Las rutas del workflow son relativas a esta raíz. El workflow construye y comprueba el site en las propuestas de cambio; publica los cambios de `main` y permite una ejecución manual desde Actions.

Para activar la publicación:

1. En el repositorio de GitHub, entra en ***Settings → Pages** y selecciona ***GitHub Actions** como origen.
2. Incorpora al repositorio el contenido de ALUMNADO, incluida la carpeta oculta `.github`, sin añadir una carpeta ALUMNADO exterior. No subes `site/`, `web_docs/` ni los entornos virtuales.
3. Envía los cambios a `main` y comprueba el resultado de ***Site del alumnado *· Zensical** en Actions.
4. Abre la URL que muestro el despliegue y prueba una descarga de cada tipo.

La preparación local no activa Pages ni envía cambios a GitHub. Si cambia el repositorio o se usa un dominio propio, actualiza `site_url` antes de publicar.

Referencia: [publicació amb GitHub Actions en la documentació oficial de Zensical](https://zensical.org/docs/publish-your-site/).

