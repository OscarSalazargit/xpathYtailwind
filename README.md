# xpathYtailwind
# Scraper con JavaScript y XPath

Este proyecto permite extraer información de una página web utilizando JavaScript y expresiones XPath. Funciona correctamente en un entorno local, pero presenta problemas cuando se sube a **GitHub Pages**.

## 🚨 ¿Por qué no funciona en GitHub Pages?
GitHub Pages es un servicio estático, lo que significa que solo puede servir archivos HTML, CSS y JavaScript. Cuando intentamos hacer `fetch(url)` para obtener el HTML de una página externa, el navegador bloquea la solicitud debido a las **restricciones de CORS (Cross-Origin Resource Sharing)**. 

Por seguridad, los navegadores no permiten realizar peticiones `fetch` a dominios externos a menos que el servidor de destino lo permita explícitamente con `Access-Control-Allow-Origin: *`.

## 🖥️ ¿Por qué funciona en local?
Cuando ejecutamos el código en local, en muchas ocasiones no se aplican las mismas restricciones de CORS (especialmente si trabajamos con archivos locales y no desde un servidor). Sin embargo, cuando se ejecuta en un dominio como `github.io`, las políticas de seguridad del navegador se aplican con más rigor.

## ✅ ¿Cómo solucionarlo?
Aquí hay algunas soluciones para que el código funcione incluso en GitHub Pages:

### 1️⃣ Usar un servidor proxy (CORS Anywhere)
Se puede utilizar un servicio como **CORS Anywhere**, que actúa como intermediario entre nuestra aplicación y la página que queremos scrapear.

**Ejemplo:**
```js
const response = await fetch(`https://cors-anywhere.herokuapp.com/${url}`);
```
**Desventaja:** Puede ser lento o tener limitaciones en su uso gratuito.

---

### 2️⃣ Usar una API de terceros
Servicios como [ScraperAPI](https://www.scraperapi.com/) o [ScrapingBee](https://www.scrapingbee.com/) permiten obtener el HTML de una página web sin restricciones de CORS.

**Ejemplo con ScraperAPI:**
```js
const response = await fetch(`https://api.scraperapi.com?api_key=TU_API_KEY&url=${encodeURIComponent(url)}`);
```
**Desventaja:** Algunos de estos servicios son de pago o tienen límites gratuitos.

---

### 3️⃣ Hacer el scraping desde el backend
Si tienes acceso a un servidor backend (por ejemplo, con PHP o Laravel), puedes hacer la petición desde ahí y devolver el HTML a tu frontend en GitHub Pages.

**Ejemplo con PHP:**
```php
<?php
header('Access-Control-Allow-Origin: *');
$url = $_GET['url'] ?? '';
if (filter_var($url, FILTER_VALIDATE_URL)) {
    echo file_get_contents($url);
} else {
    echo 'URL no válida';
}
?>
```
Luego, en JavaScript:
```js
const response = await fetch(`https://tu-servidor.com/scraper.php?url=${encodeURIComponent(url)}`);
```
**Desventaja:** Necesitas un servidor propio.

---

## 🚀 Conclusión
Si quieres que el scraper funcione en GitHub Pages, necesitas usar una de las soluciones mencionadas. La mejor opción dependerá de tus necesidades y recursos. Si solo estás probando, CORS Anywhere puede servir. Para un proyecto más estable, es recomendable un backend propio o una API de scraping.

📌 **Recuerda:** Hacer scraping en algunas páginas puede estar restringido por sus términos de uso. ¡Úsalo con responsabilidad! 😉
