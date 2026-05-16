Análisis de Tráfico de Red: Detección de Beaconing en DNS y HTTP
Resumen Ejecutivo

El presente proyecto documenta un laboratorio de ciberseguridad orientado al análisis de tráfico de red, con el objetivo de identificar patrones de comunicación automatizada compatibles con comportamiento de tipo beaconing.

El análisis se llevó a cabo en un entorno controlado, donde el tráfico fue generado intencionalmente, capturado mediante tcpdump y posteriormente analizado con Wireshark, simulando el flujo de trabajo de un analista SOC.

Entorno del Laboratorio

Equipo de análisis:
Kali Linux — 192.168.93.128

Equipo objetivo:
Ubuntu Server — 192.168.93.130

Servidor DNS:
192.168.93.2

Red:
192.168.93.0/24

Herramientas utilizadas:

tcpdump
Wireshark
curl
nslookup
Objetivos
Capturar tráfico de red en un entorno controlado
Analizar comunicaciones DNS y HTTP
Identificar patrones repetitivos de tráfico saliente
Simular un escenario de análisis SOC
Definir lógica de detección basada en el comportamiento observado
Simulación de Tráfico

El tráfico fue generado desde el equipo objetivo con el fin de emular comportamiento de comunicación saliente.

Se utilizaron las siguientes herramientas:

Generación de solicitudes HTTP:

curl http://example.com

Resolución de nombres de dominio:

nslookup example.com

Simulación de tráfico repetitivo (beaconing):

while true; do curl http://example.com; sleep 2; done

Captura de Tráfico

La captura del tráfico de red se realizó desde el equipo de análisis mediante el siguiente comando:

sudo tcpdump -i eth0 -w network-analysis.pcap

El archivo resultante contiene el tráfico utilizado para el análisis posterior.

Análisis
Análisis DNS

Se identificaron múltiples consultas DNS hacia dominios externos, entre ellos:

example.com
neverssl.com

Las consultas fueron realizadas contra el servidor DNS interno (192.168.93.2) e incluyen registros de tipo A y AAAA.

Este comportamiento es consistente con procesos normales de resolución de nombres.

Análisis HTTP

Mediante el uso de Wireshark y el filtro:

http.request

Se observaron múltiples solicitudes HTTP GET dirigidas a direcciones IP externas:

172.66.147.243
104.20.23.154

Las solicitudes presentan características consistentes:

Método: GET
Protocolo: HTTP/1.1
User-Agent: curl
Identificación de Patrones

El análisis del tráfico permitió identificar:

Solicitudes HTTP repetitivas
Intervalos de tiempo regulares entre conexiones
Comunicación hacia destinos constantes

Este patrón es característico de comportamientos automatizados y puede asociarse a:

Beaconing
Scripts automatizados
Comunicación de Command and Control (C2)
Hallazgos
Repetición de consultas DNS hacia los mismos dominios
Tráfico HTTP periódico hacia direcciones IP externas
Generación automatizada de solicitudes
Uso de protocolo HTTP sin cifrado

El conjunto de estos indicadores evidencia un patrón compatible con actividad de tipo beaconing.

Mapeo MITRE ATT&CK
Táctica: Command and Control
Técnica: T1071 – Application Layer Protocol
Subtécnica: T1071.001 – Web Protocols
Lógica de Detección

Se propone la siguiente lógica de detección:

Múltiples solicitudes HTTP
Desde una misma dirección IP de origen
Hacia un mismo destino
En intervalos de tiempo reducidos y constantes

Este patrón puede ser implementado en soluciones SIEM para la generación de alertas.

Recomendaciones
Implementar monitoreo continuo del tráfico de red
Configurar reglas de correlación en SIEM para detectar patrones repetitivos
Analizar logs DNS en busca de comportamientos anómalos
Restringir conexiones salientes innecesarias
Implementar inspección de tráfico y controles de red
Evidencia

Las evidencias utilizadas en el análisis se encuentran disponibles en el directorio:

/evidence

Incluyen:

Generación de tráfico HTTP
Resolución DNS
Captura de tráfico
Análisis en Wireshark
Flujo de red filtrado por dirección IP
Informe Completo

El informe técnico detallado se encuentra disponible en:

/report/Incident-Report-Network-Traffic-Analysis.pdf

Autor

Nicolás Sotomayor
SOC Analyst (Junior) | Blue Team | Cybersecurity
