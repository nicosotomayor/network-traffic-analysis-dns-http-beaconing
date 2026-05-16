
# Informe de Análisis de Tráfico de Red

## Identificación de Patrones de Beaconing mediante DNS y HTTP

---

## 1. Resumen Ejecutivo

El presente informe expone los resultados del análisis de tráfico de red realizado en un entorno controlado, con el objetivo de identificar patrones de comunicación anómalos.

A partir de la evidencia recolectada (ver Anexo A), se detectaron consultas DNS repetitivas y solicitudes HTTP periódicas hacia destinos externos. Este comportamiento es consistente con patrones de beaconing, típicamente asociados a mecanismos de Command and Control (C2).

---

## 2. Alcance y Objetivos

### Alcance

El análisis se realizó sobre tráfico capturado en un entorno de laboratorio simulado, documentado mediante evidencia técnica incluida en el Anexo A.

### Objetivos

* Analizar tráfico DNS y HTTP
* Identificar patrones repetitivos de comunicación
* Correlacionar eventos de red
* Generar evidencia técnica documentada

---

## 3. Descripción del Entorno

### Infraestructura

* Host Analista: Kali Linux (192.168.93.128) — ver Imagen 04
* Host Objetivo: Ubuntu Server (192.168.93.130) — ver Imagen 11
* Red: 192.168.93.0/24

---

## 4. Metodología

Se generó tráfico controlado desde el host objetivo utilizando herramientas de línea de comandos.

### Evidencia

* Generación de solicitud HTTP mediante curl — ver Imagen 01
* Resolución DNS de dominio externo — ver Imagen 02
* Ejecución repetitiva de solicitudes (simulación de automatización) — ver Imagen 03

---

## 5. Captura de Tráfico

La captura fue realizada mediante tcpdump:

sudo tcpdump -i eth0 -w network-analysis.pcap

### Evidencia

* Inicio de captura — ver Imagen 06
* Resumen de paquetes capturados — ver Imagen 07

---

## 6. Análisis de Tráfico

### 6.1 Actividad DNS

Se identificaron consultas DNS hacia dominios externos.

### Evidencia

* Análisis DNS en Wireshark — ver Imagen 08

### Observaciones

* Consultas repetitivas a example.com
* Respuestas desde servidor DNS interno

---

### 6.2 Actividad HTTP

Se identificaron múltiples solicitudes HTTP salientes.

### Evidencia

* Tráfico HTTP en Wireshark — ver Imagen 09

### Observaciones

* Solicitudes GET repetitivas
* Destinos externos identificados
* Uso de herramienta curl

---

### 6.3 Flujo de Comunicación

Se analizó el comportamiento del host objetivo.

### Evidencia

* Análisis de tráfico por IP origen — ver Imagen 10
* Validación de conectividad — ver Imagen 05

---

## 7. Hallazgos

A partir de la correlación de la evidencia (Anexo A), se identificó:

* Comunicación periódica hacia destinos externos
* Consultas DNS repetidas
* Tráfico HTTP automatizado
* Patrón consistente de beaconing

---

## 8. Mapeo MITRE ATT&CK

* Táctica: Command and Control
* Técnica: T1071 – Application Layer Protocol
* Subtécnica: T1071.001 – Web Protocols

---

## 9. Conclusión

La evidencia analizada confirma la presencia de un patrón de comunicación automatizada que simula comportamiento de beaconing.

Este tipo de actividad es relevante en la detección de amenazas avanzadas y puede ser identificado mediante monitoreo de tráfico de red y correlación de eventos.

---

## 10. Anexo A – Evidencia

| Imagen | Descripción                            |
| ------ | -------------------------------------- |
| 01     | Generación de solicitud HTTP (curl)    |
| 02     | Resolución DNS (nslookup)              |
| 03     | Automatización de solicitudes          |
| 04     | Configuración de red en Kali           |
| 05     | Validación de conectividad (ping)      |
| 06     | Inicio de captura con tcpdump          |
| 07     | Resumen de captura                     |
| 08     | Análisis DNS en Wireshark              |
| 09     | Análisis HTTP en Wireshark             |
| 10     | Flujo de tráfico por IP                |
| 11     | Configuración de red del host objetivo |

---





