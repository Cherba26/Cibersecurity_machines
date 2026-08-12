`dificultad: easy`
`tema: Network Forensics`

Como datos para el laboratorio tenemos una captura .pcap que analizaremos con wireshark.

### Q1. The attacker successfully executed a command to download the first stage of the malware. What is the URL from which the first malware stage was installed?
`Respuesta: http://45.126.209.4:222/mdm.jpg`

Filtraremos en Wireshark por *http* para ver como se comunicó el atacante para descargar el payload.
El primer GET (xlm.txt) no corresponde a la respuesta puesto que es un script creado para la descarga autentica del malware (mdm.jpg).

![](</Imagenes/image1.png>)

![](</Imagenes/image2.png>)

### Q2. Which hosting provider owns the associated IP address?
`Respuesta: ReliableSite.Net`

Buscamos en abuseipdb.com la ip de la web.

![](</Imagenes/image3.png>)

### Q3.  By analyzing the malicious scripts, two payloads were identified: a loader and a secondary executable. What is the SHA256 of the malware executable?
`Respuesta: 1eb7b02e18f67420f42b1d94e74f3b6289d92672a0fb1786c30c03d68e81d798`

Observamos en el intercambio de mensajes del comando GET la tranmiosión de una imagen en código hexadecimal.

![](</Imagenes/image4.png>)

Vamos  a Cyberchef donde traduciremos el texto hexadecimal y buscaremos por hash MD5 ya que no esta SHA256.

![](</Imagenes/image5.png>)

Posteriormente buscamos este mismo hash MD5 en virustotal para poder obtener el hash SHA256 que nos piden.

![](</Imagenes/image6.png>)


### Q4. What is the malware family label based on Alibaba?
`Respuesta: AsyncRat`

![](</Imagenes/image7.png>)

Dentro de virustotal, el antivirus Alibaba detecta la etiqueta AsyncRat.

### Q5.  What is the PE header compile (Creation Time) timestamp of the malware?
`Respuesta: 2023-10-30 15:08`
![](</Imagenes/image8.png>)

Virustotal nos ofrece la información.

### Q6. Which LOLBin is leveraged for stealthy process execution in this script? Provide the full path.
`Respuesta:C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegSvcs.exe`

En Wireshark analizando la transmisión de datos.
![](</Imagenes/image9.png>)

Con cyberchef aplicamos reemplazo.

![](</Imagenes/image10.png>)

### Q7.  The script is designed to drop several files. List the names of the files dropped by the script.
`Respuesta: Conted.ps1,Conted.bat,Conted.vbs`

Analizando  en Wireshark, se muestra, distintas rutas donde guardan dichos archivos.

```bash
'@

[IO.File]::WriteAllText("C:\Users\Public\Conted.ps1", $Content)
'@

[IO.File]::WriteAllText("C:\Users\Public\Conted.bat", $Content)
'@

[IO.File]::WriteAllText("C:\Users\Public\Conted.vbs", $Content)
```
