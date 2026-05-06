<img width="1118" height="534" alt="Scan nmap -sV" src="https://github.com/user-attachments/assets/8d7bb459-02ca-428b-b8a3-b99bb221db98" />
# 🛡️ Laboratorio SOC: Detección y Análisis de Enumeración

## 🎯 Objetivo del Laboratorio
Demostrar de forma práctica la criticidad de la fase de enumeración en un ciberataque. Este ejercicio simula el reconocimiento activo contra una infraestructura vulnerable para mapear su superficie de ataque, validando conceptos teóricos de gestión de riesgos y Hardening.

## 🛠️ Entorno de Simulación
* **Atacante (Red Team):** Kali Linux.
* **Objetivo (Blue Team):** Servidor Linux vulnerable (Metasploitable 2) aislado en red NAT.
* **Herramienta Principal:** Nmap (Motor de escaneo y detección de versiones).

---

## 🔍 Análisis de la Evidencia (Escaneo de Puertos)
*(La captura superior muestra la ejecución del comando `nmap -sV 172.16.220.134`)*

Al interactuar con el sistema objetivo, Nmap logró mapear una superficie de ataque extremadamente amplia. Los hallazgos más críticos que facilitan la explotación son:

1. **Identificación Precisa de Versiones:** El escaneo no solo reveló puertos abiertos, sino el software subyacente. Por ejemplo, el **Puerto 21 (FTP)** corre `vsftpd 2.3.4`, una versión históricamente conocida por contener vulnerabilidades explotables de ejecución remota de comandos.
2. **Exposición de Servicios Internos:** Los puertos **139 y 445 (Samba smbd 3.X)** están expuestos. En un entorno real (hospitalario o industrial), esto indica que un atacante podría enumerar usuarios y recursos compartidos de red, un paso fundamental para el movimiento lateral y el robo de credenciales.
3. **Servicios Obsoletos / Inseguros:** Presencia de servicios en texto plano como **Telnet (23)**, lo que permitiría la interceptación de credenciales mediante *sniffing* si un administrador intenta loguearse en esa máquina.

---

## 🛡️ Propuesta de Contención y Mitigación Integral
Basado en los marcos de trabajo **ISO/IEC 27001** y **NIST CSF 2.0**, la mitigación de esta amenaza requiere las siguientes acciones inmediatas:

* **Reducción de la Superficie de Ataque (Hardening):** Deshabilitar inmediatamente servicios innecesarios o no cifrados como Telnet y FTP, forzando el uso de alternativas seguras como SSH (Puerto 22) con autenticación basada en llaves públicas, no en contraseñas.
* **Control de Red:** Configurar el Firewall perimetral e interno para bloquear escaneos masivos y limitar el acceso al puerto 445 (Samba) únicamente a las subredes de administración autorizadas.
* **Gestión de Parches:** Actualizar urgentemente los servicios expuestos a versiones estables y soportadas para evitar el uso de *exploits* públicos ya conocidos.
