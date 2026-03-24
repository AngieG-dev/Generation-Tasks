## **🧩 1\. ¿Qué es una dirección IP?**

Una dirección IP es un identificador único que permite que los dispositivos en una red se encuentren y se comuniquen entre sí, permitiendo el enrutamiento de paquetes de datos.

| Concepto | Definición Simple | Nivel Técnico Breve | Ejemplo |
| :---- | :---- | :---- | :---- |
| **IP** | El "número" o "dirección postal" que identifica a un dispositivo en una red. | Identificador único en la **Capa 3 (Red)** del modelo OSI para enrutamiento. | 192.168.1.10 |
| **IPv4** | Formato clásico de 4 grupos de números separados por puntos. | Usa **32 bits**, limitando el espacio a \~4.3 mil millones de direcciones. | 8.8.8.8 |
| **IPv6** | Nueva versión con letras y números para evitar que se agoten las direcciones. | Usa **128 bits**, permitiendo direcciones casi ilimitadas en formato hexadecimal. | 2001:0db8::1 |

---

## **🏷️ 2\. Tipos de IP (Clasificación para Desarrollo)**

Para gestionar servidores y bases de datos, es vital distinguir cómo se comportan estas direcciones según su visibilidad y persistencia.

* **Pública vs. Privada:**  
  * **Pública:** Visible en todo Internet; es la IP que el mundo usa para encontrar tu servidor o API. Ejemplo: El backend de una app en AWS.  
  * **Privada:** Usada dentro de redes locales (LAN) como tu casa u oficina. No es accesible desde fuera, permitiendo que tus dispositivos internos se hablen de forma segura.  
* **Estática vs. Dinámica:**  
  * **Estática:** Es una IP fija que **no cambia**. Es indispensable para servidores web, bases de datos y servicios DNS que deben ser encontrados siempre en el mismo sitio.  
  * **Dinámica:** Asignada automáticamente y **cambia periódicamente** o al reiniciar el equipo. Es la que suelen asignar los proveedores (ISP) a usuarios domésticos para optimizar costos.

## **💻 3\. Conexión Directa con Desarrollo (Crítico)**

El manejo de IPs define cómo se conectan los componentes de tu arquitectura (Frontend, Backend, DB).

## **El Entorno Local (localhost)**

* **IP de Loopback:** 127.0.0.1 es la dirección de "sí mismo".  
* **Restricción:** Solo es accesible desde tu propia máquina. No puedes acceder al localhost de otro computador porque esa IP siempre apunta al equipo que hace la petición.  
* **Servidores de Desarrollo:** Al ejecutar npm run dev, el servidor suele escuchar en localhost:3000. Si quieres que otros en tu red vean tu avance, debes configurar el servidor para que escuche en 0.0.0.0 (todas las interfaces) y compartir tu **IP Privada**.

## **Infraestructura en la Nube**

* **Bases de Datos:** Servicios como AWS RDS usan **Endpoints** (nombres de dominio) que resuelven a IPs privadas dentro de una VPC para seguridad, o IPs públicas protegidas por un **Firewall**.  
* **Producción:** Se utilizan IPs públicas directas o nombres de dominio gestionados por **DNS**.

## **🛠️ 4\. Caso Práctico: Depuración de Conectividad**

Un error de IP puede detener un proyecto entero aunque el código esté perfecto.

**Escenario:** El frontend funciona localmente pero falla al conectarse al backend en producción.

Ejemplo:

* Frontend apunta a `localhost:3000`  
* Pero backend está en otro servidor → ❌ no conecta


| Qué revisar | Detalle del problema |
| :---- | :---- |
| **URL del Backend** | Verificar si el código aún apunta a localhost en lugar de la IP pública o dominio real. |
| **CORS** | La IP del frontend no está en la lista de permitidos del backend. |
| **Binding de Red** | El backend está configurado en 127.0.0.1 (solo él se ve) en lugar de 0.0.0.0. |
| **Security Groups** | Reglas de firewall (especialmente en AWS) bloqueando el puerto de entrada. |

### **⚠️ Errores típicos**

* ❌ Usar `localhost` en producción  
* ❌ IP privada desde internet  
* ❌ Puerto cerrado  
* ❌ Backend escuchando en `127.0.0.1` en vez de `0.0.0.0`  
* ❌ Security groups (AWS) mal configurados

## **🗺️ 5\. Los Facilitadores: DHCP y DNS**

Estos protocolos automatizan la gestión manual de números IP.

* **DHCP:** Asigna automáticamente una IP a tu dispositivo al conectarse a la red. Sin él, tendrías que configurar cada equipo manualmente.  
* **DNS:** Es la "guía telefónica" de Internet que traduce nombres (ej: google.com) a IPs (ej: 142.250.x.x).

**Flujo de navegación:**

1. Escribes el nombre en el navegador.  
2. Tu PC consulta su **caché** o pregunta al servidor **DNS**.  
3. El DNS devuelve la IP correspondiente.  
4. El navegador se conecta a esa IP y el servidor entrega la web.

<img width="907" height="55" alt="image" src="https://github.com/user-attachments/assets/4aacea22-e8f5-4967-8886-882fdbdbf47d" />

## **🌟 6\. Infraestructura Avanzada**

* **NAT (Network Address Translation):** Permite que muchos dispositivos en una oficina compartan una sola IP pública para navegar, traduciendo las peticiones internas.  
* **Docker:** Crea una **red virtual** donde cada contenedor tiene su IP interna. Los contenedores se comunican entre sí por nombre o puertos expuestos.  
* **AWS EC2:** Las instancias tienen IP privada y pública. La IP pública cambia si reinicias la instancia, a menos que uses una **Elastic IP** (estática).

## **🎭 7\. Analogías Obligatorias**

* **IP \=** Dirección física de una casa.  
* **DNS \=** Agenda de contactos (buscas el nombre para obtener el número).  
* **DHCP \=** Recepcionista que te asigna una habitación apenas llegas al hotel.

# **🧠 BONUS (nivel pro)**

### **🔹 ¿Qué es NAT?**

Network Address Translation:

* Permite que múltiples dispositivos con IP privada compartan una IP pública.  
* Clave para internet doméstico.

---

### **🔹 IP en Docker**

* Cada contenedor tiene su propia IP interna  
* Docker crea una red virtual  
* Se accede por:  
  * nombre del contenedor  
  * o puertos expuestos (`localhost:3000`)

---

### **🔹 IP en AWS (EC2)**

* Instancia tiene:  
  * IP privada (interna)  
  * IP pública (internet)  
* Puede cambiar si no es “Elastic IP”

## 

## **📖 Glosario de Términos Técnicos**

* **Binding:** Proceso de asociar un servicio (como tu backend) a una dirección IP y puerto específicos para escuchar peticiones.  
* **Capa 3 (OSI):** El nivel del modelo de red encargado del direccionamiento y envío de datos entre redes distintas.  
* **CORS (Cross-Origin Resource Sharing):** Mecanismo de seguridad que permite o restringe peticiones entre diferentes dominios o IPs.  
* **Elastic IP:** Dirección IP estática y pública proporcionada por AWS que no cambia tras reinicios de instancia.  
* **Endpoint:** Punto de conexión o URL final de un servicio, como una API o base de datos.  
* **Firewall:** Barrera de seguridad que permite o bloquea el tráfico de red basado en reglas (como IPs o puertos).  
* **Hexadecimal:** Sistema numérico de base 16 (0-9 y A-F) usado para simplificar la lectura de direcciones IPv6.  
* **ISP (Internet Service Provider):** Empresa que te brinda acceso a Internet.  
* **LAN (Local Area Network):** Red de computadoras que abarca un área pequeña, como una casa.  
* **Loopback:** Interfaz virtual que permite a un equipo enviarse tráfico a sí mismo (127.0.0.1).  
* **NAT:** Técnica que traduce direcciones privadas a una pública para compartir conexión.  
* **Puerto:** Un número que identifica un canal de comunicación específico dentro de una IP (ej: 80 para web, 3000 para desarrollo).  
* **VPC (Virtual Private Cloud):** Una sección aislada lógicamente de la nube de AWS donde puedes lanzar recursos en una red virtual definida por ti.

[image1]: <data:image/png;base64,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>
