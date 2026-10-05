# 📘 Manual Definitivo del Servidor Inteligente: Desde Cero Absoluto

Este documento es una guía explicativa diseñada para que **cualquier persona, incluso si nunca en su vida ha usado un computador**, pueda entender cómo funciona el servidor inteligente de esta casa. 
Aquí te explicaremos qué es cada pieza, para qué sirve, por qué se instaló, y cómo se solucionaron los problemas técnicos de forma detallada.

---

## 📚 1. Conceptos Básicos: Entendiendo la Tecnología

Para entender este servidor, primero debemos conocer algunas palabras clave:

### ¿Qué es Linux Debian?
Imagina que Windows es un auto automático de ciudad, fácil de manejar pero cerrado. **Linux Debian** es como un camión de carga industrial: no tiene radio ni aire acondicionado (interfaz gráfica), se maneja por comandos de texto, pero es capaz de estar encendido por 10 años sin fallar, no tiene virus y es gratuito. Es el sistema operativo (el alma) de nuestro servidor.

### ¿Qué son Docker y Docker Compose?
Antes, si instalabas 30 programas en un computador, terminaban peleando entre ellos por la memoria o se desconfiguraban. 
- **Docker:** Imagina un barco de carga gigante. En lugar de tirar toda la mercancía suelta en la cubierta, Docker pone cada programa dentro de su propio **"Contenedor"** de metal sellado. Cada contenedor tiene adentro todo lo que el programa necesita para vivir. Si un programa se vuelve loco o se rompe, se queda atrapado en su contenedor y no daña al resto del computador.
- **Docker Compose:** Es como el manual de instrucciones para la grúa del puerto. Es un archivo de texto (`docker-compose.yml`) donde escribimos una lista de todos los contenedores que queremos. Con un solo comando (`docker compose up -d`), la grúa lee el manual y arma todos los contenedores de golpe.

#### ¿Cómo se instala Docker desde cero?
Si un día tienes un computador vacío con Debian, se instala ejecutando estos comandos exactos en la pantalla negra (terminal):
```bash
# 1. Actualizar el sistema
sudo apt-get update
# 2. Descargar las herramientas de instalación
sudo apt-get install ca-certificates curl
# 3. Descargar la llave oficial de Docker
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
# 4. Agregar Docker a la tienda de aplicaciones del sistema
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# 5. Instalar Docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## 🏭 2. El Ecosistema: Los Habitantes del Servidor

El servidor es como un edificio, y en su interior viven **29 contenedores**. Cada uno hace un trabajo específico por el que, si contrataras servicios en internet, pagarías cientos de dólares al mes.

### 🎬 Entretenimiento y Multimedia
1. **Jellyfin:** Es nuestro **Netflix privado**. Organiza todas las películas y series que descargamos y las envía a los televisores o celulares de la casa.
2. **Prowlarr:** Es el "Buscador Maestro". Tú le dices qué quieres ver, y él busca en cientos de páginas web al mismo tiempo.
3. **Sonarr:** El "Asistente de Series". Tú le dices "Quiero ver Los Simpson", y él automáticamente vigila internet, descarga cada capítulo nuevo que sale, le cambia el nombre, le pone la carátula y se lo entrega a Jellyfin.
4. **Radarr:** Hermano de Sonarr, pero exclusivo para **Películas**.
5. **qBittorrent / SABnzbd:** Son los "Camiones de Reparto". Sonarr y Radarr les pasan los enlaces, y estos programas se encargan de hacer el trabajo sucio de descargar los archivos grandes desde internet.

### 🛡️ Seguridad y Redes
6. **AgentDVR:** El "Guardia de Seguridad". Se conecta a todas las cámaras de la casa, detecta movimiento, graba los videos y te permite verlos en vivo desde tu celular.
7. **AdGuard Home:** El "Filtro de Agua" del internet. Se encarga de asignarle un número (IP) a cada celular de tu casa, y de paso, bloquea toda la publicidad y rastreadores de las páginas web antes de que lleguen a tu pantalla.

### ☁️ Productividad y Nube
8. **Nextcloud:** Nuestro **Google Drive privado**. Sirve para guardar documentos, archivos de trabajo y compartirlos con otras personas, sin depender de empresas externas.
9. **Immich:** Nuestro **Google Fotos privado**. Respalda automáticamente las fotos de tu celular, reconoce caras usando Inteligencia Artificial, y crea mapas de dónde tomaste las fotos.
10. **Vaultwarden:** La **Caja Fuerte**. Un administrador que guarda todas tus contraseñas bajo cifrado militar.

### ⚙️ Herramientas del Sistema
11. **Homarr:** El "Tablero de Control". Es la página web bonita llena de botones desde donde controlamos toda la casa.
12. **Portainer:** El "Jefe del Puerto". Una interfaz gráfica que nos permite ver si los contenedores de Docker están encendidos o apagados, sin tener que usar código.
13. **Home Assistant:** El "Mayordomo". Conecta luces inteligentes, enchufes y sensores de la casa para automatizarlos.
14. **Uptime Kuma:** El "Doctor". Un programa que revisa cada minuto si las demás aplicaciones están funcionando. Si algo se cae, te avisa.
15. **Duplicati:** El "Seguro de Vida". Hace copias de seguridad automáticas de todo lo importante para no perder datos si se rompe el disco duro.
16. **RomM:** Administrador de videojuegos retro. Te permite tener tu colección de Nintendo/PlayStation organizada.
17. **FileBrowser:** Un explorador de archivos web (como la carpeta "Mis Documentos") para ver qué hay dentro del servidor desde cualquier navegador.

---

## 🛠️ 3. Historias de Batalla: Problemas y Soluciones Técnicas

Para que todo esto funcionara, hubo que vencer varios obstáculos tecnológicos graves. Aquí explicamos cómo se logró:

### Batalla 1: El Límite de Dispositivos (Doble-NAT)
* **Teoría del Problema:** La compañía de internet (Fiberhome) nos puso un router que solo soportaba 8 dispositivos WiFi. Al conectar las 8 cámaras, los celulares se quedaban sin internet.
* **Solución Técnica:** Instalamos un "Escudo". Pusimos un Router Xiaomi con un sistema libre (OpenWrt). El Xiaomi se conectó al Fiberhome tomando solo **1 espacio**. Luego, el Xiaomi creó su propia red wifi invisible (`192.168.2.X`) y conectó a las 8 cámaras allí. 
* **La Lógica (Comandos):** Para que nuestro servidor viera esas cámaras ocultas, tuvimos que enseñarle un "mapa" en Debian:
  `sudo ip route add 192.168.2.0/24 via 192.168.1.2`
  *(Significa: Si quieres hablar con la red de las cámaras 2.0, cruza por la puerta del Xiaomi en la 1.2).*

### Batalla 2: El Estrangulamiento Térmico (Servidor a 90°C)
* **Teoría del Problema:** Los videos modernos vienen ultra-comprimidos (formato HEVC/H.265) para pesar menos. Descomprimirlos requiere un chip especial. Nuestro procesador antiguo no lo tiene, así que al intentar ver una película, el procesador se forzaba tanto que el computador alcanzaba los 90 grados Celsius.
* **Solución Técnica:** 
  1. Le dijimos a Jellyfin que tradujera los videos de a poco ("Throttling"), con pausas para enfriar el sistema.
  2. Creamos un Script en lenguaje **Python** que hackeó la base de datos de Sonarr y Radarr. 
* **La Lógica (El Script):** Le dijimos a la base de datos que "odiara" el formato HEVC, dándole una puntuación de `-10000`. Al correr el script, el sistema borró las películas pesadas y automáticamente comenzó a buscar y descargar formatos clásicos (H.264) que no calientan el equipo.

### Batalla 3: El Muro de Pago de las Cámaras (AgentDVR)
* **Teoría del Problema:** Cuando salías a la calle, usabas nuestra VPN (Tailscale) para entrar seguro al servidor. Pero el programa de cámaras (AgentDVR) detectaba que tu conexión venía desde afuera, y ponía una pantalla negra que decía: *"Para ver desde afuera de su casa, pague una suscripción"*.
* **Solución Técnica:** Engañamos a AgentDVR. Instalamos una herramienta llamada `socat` que actúa como un túnel. 
* **La Lógica:** Creamos un servicio del sistema (`agent-proxy.service`) que intercepta tu conexión VPN y se la entrega a las cámaras "disfrazada" como si el computador se estuviera conectando a sí mismo (`127.0.0.1`). Así, AgentDVR cree que estás físicamente frente al servidor y te da acceso gratis de por vida.

### Batalla 4: Hackeando el Panel Visual (Homarr)
* **Teoría del Problema:** Faltaban botones en nuestra página de control Homarr (faltaba el Router Xiaomi, RomM y FileBrowser). La nueva versión de Homarr guarda sus botones en una base de datos muy compleja y cerrada (`SQLite`).
* **Solución Técnica:** Usando comandos de inyección de código (Lenguaje **SQL**), abrimos a la fuerza la base de datos.
* **La Lógica:** Insertamos código matemático que decía exactamente qué icono usar y en qué coordenadas de la pantalla dibujar el botón. 
  ```sql
  -- Ejemplo de inyección para forzar el dibujo del botón del router:
  INSERT INTO item_layout (item_id, layout_id, x_offset, y_offset, width, height) 
  VALUES ('item_xiaomi', 'tablero_principal', 10, 10, 2, 2);
  ```
  *(Significa: Dibuja el botón a 10 cuadros de la izquierda, 10 desde arriba, con un tamaño de 2x2).*

---
**Conclusión:**
Este servidor dejó de ser un simple computador; hoy es un ecosistema inteligente, enlazado quirúrgicamente. Descarga contenido solo, te protege de publicidad, te da cámaras de vigilancia sin licencias corporativas y mantiene tus archivos seguros, todo de forma invisible.

### Batalla 5: El Fantasma de Jellyfin
* **Teoría del Problema:** Habíamos eliminado físicamente la carpeta de "CCTV Grabaciones" que enlazaba los videos a la televisión, pero Jellyfin es un sistema que almacena un caché estricto. Por ende, seguía mostrando en la pantalla de la televisión un cuadro azul de una biblioteca que ya no existía.
* **Solución Técnica:** Se accedió al motor interno de bases de datos del sistema multimedia (Jellyfin).
* **La Lógica:** Mediante una inyección SQL directa a `jellyfin.db`, se localizó el identificador único (`CollectionFolder`) de CCTV Grabaciones en la tabla `BaseItems` y se eliminó de raíz usando un comando `DELETE`. Tras reiniciar la memoria RAM del contenedor, el cuadro azul desapareció del televisor permanentemente.
