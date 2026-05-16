# Informe de Análisis de Tráfico de Red

## Identificación de Patrones de Beaconing mediante DNS y HTTP

---

## 1. Resumen Ejecutivo

El presente informe expone los resultados de un análisis de tráfico de red realizado en un entorno controlado, con el objetivo de identificar patrones anómalos de comunicación saliente.

A partir de la captura y análisis de paquetes, se detectaron consultas DNS repetitivas y solicitudes HTTP periódicas. Este comportamiento es consistente con patrones de beaconing, comúnmente asociados a comunicaciones de Command and Control (C2).

Los hallazgos evidencian oportunidades de detección a nivel de red y demuestran cómo este tipo de actividad puede ser identificada mediante técnicas utilizadas en Centros de Operaciones de Seguridad (SOC).

---

## 2. Alcance y Objetivos

### Alcance

El análisis se llevó a cabo en un entorno de laboratorio simulado, compuesto por un equipo analista y un host objetivo generador de tráfico.

### Objetivos

* Capturar y analizar tráfico de red en un entorno controlado
* Identificar patrones de comunicación DNS y HTTP
* Detectar actividad repetitiva saliente
* Simular un flujo de análisis típico de un SOC
* Definir criterios de detección aplicables a SIEM

---

## 3. Descripción del Entorno

### Infraestructura

* Host Analista: Kali Linux (192.168.93.128)
* Host Objetivo: Ubuntu Server (192.168.93.130)
* Servidor DNS: 192.168.93.2
* Segmento de Red: 192.168.93.0/24

### Herramientas Utilizadas

* tcpdump (captura de tráfico)
* Wireshark (análisis de paquetes)
* curl (generación de solicitudes HTTP)
* nslookup (resolución DNS)

---

## 4. Metodología de Generación de Tráfico

Se generó tráfico saliente desde el host objetivo con el fin de simular patrones realistas de comunicación.

Las acciones realizadas incluyen:

* Solicitudes HTTP a dominios externos mediante curl
* Resolución de dominios utilizando nslookup
* Ejecución repetitiva de solicitudes para simular comportamiento automatizado

Se implementó un mecanismo de ejecución en bucle para generar intervalos constantes, replicando un patrón de beaconing.

---

## 5. Captura de Tráfico

El tráfico fue capturado desde el host analista mediante el siguiente comando:

sudo tcpdump -i eth0 -w network-analysis.pcap

El archivo resultante contiene tráfico DNS y HTTP generado durante la simulación y constituye la base del análisis.

---

## 6. Análisis y Hallazgos

### 6.1 Actividad DNS

Se identificaron múltiples consultas DNS dirigidas a dominios externos, entre ellos:

* example.com
* neverssl.com

Observaciones clave:

* Consultas repetitivas a los mismos dominios
* Uso del servidor DNS interno (192.168.93.2)
* Resolución de registros tipo A y AAAA

Este comportamiento es consistente con procesos de resolución previos a comunicaciones salientes.

---

### 6.2 Actividad HTTP

Mediante el uso de filtros en Wireshark se identificó tráfico HTTP con las siguientes características:

* Solicitudes HTTP GET repetitivas
* Conexiones hacia direcciones IP externas:

  * 172.66.147.243
  * 104.20.23.154
* Estructura de solicitud uniforme
* User-Agent identificado como curl

---

### 6.3 Análisis de Patrón de Tráfico

El análisis combinado de DNS y HTTP evidencia:

* Comunicación saliente periódica
* Conexión repetida a los mismos destinos
* Estructura constante de las solicitudes
* Intervalos regulares entre eventos

Este patrón es característico de comportamiento automatizado y coincide con técnicas de beaconing.

---

## 7. Hallazgos Clave

* Consultas DNS repetitivas a dominios específicos
* Comunicación HTTP periódica hacia hosts externos
* Generación automatizada de tráfico desde el host objetivo
* Uso de protocolo HTTP sin cifrado

La correlación de estos indicadores sugiere comportamiento compatible con mecanismos de Command and Control.

---

## 8. Mapeo MITRE ATT&CK

* Táctica: Command and Control
* Técnica: T1071 – Application Layer Protocol
* Subtécnica: T1071.001 – Web Protocols

---

## 9. Consideraciones de Detección

Se recomienda implementar las siguientes reglas de detección:

* Alta frecuencia de solicitudes HTTP desde un mismo origen
* Conexiones repetidas hacia un mismo destino
* Intervalos constantes entre solicitudes
* Consultas DNS recurrentes a dominios específicos

Estas condiciones pueden ser correlacionadas en plataformas SIEM para detectar actividad sospechosa de forma temprana.

---

## 10. Conclusión

El análisis realizado permitió identificar patrones de comunicación automatizada que replican comportamiento típico de beaconing.

Este tipo de actividad representa un indicador relevante en la detección de amenazas avanzadas y destaca la importancia del monitoreo continuo del tráfico de red.

La implementación de mecanismos de detección basados en comportamiento resulta clave para fortalecer las capacidades de respuesta ante incidentes en entornos SOC.

---
