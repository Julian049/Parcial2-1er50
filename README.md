# Docker Traefik

# Autores
**Diego Alejandro Gil Otálora** Cod:202222152<br>
**Julian David Bocanegra Segura** Cod: 202220214  
Universidad Pedagógica y Tecnológica de Colombia  
Ingeniería de Sistemas y Computación - Sistemas Distribuidos  
Tunja, 2025 

# Punto 1

Editamos etc/host

<img width="401" height="123" alt="image" src="https://github.com/user-attachments/assets/a63bebfd-49e7-46e2-ae8d-330e99512942" />



# Punto 2
Se modifica el compose.yaml para que tenga 2 apis backend y backend-legendario
y se ejecuta
<img width="1777" height="864" alt="image" src="https://github.com/user-attachments/assets/68519afd-4368-44cc-bbde-8b4ac41586e6" />
Ahora se balancea solo el backendn y verificamos
<img width="1777" height="648" alt="image" src="https://github.com/user-attachments/assets/0a1774ef-535e-4e43-95a0-fce0bd74a8f5" />
Revisamos los logs de las 2 apis
<img width="1777" height="520" alt="image" src="https://github.com/user-attachments/assets/c48daa57-fab8-4225-9e47-fb8888dadf76" />

# Punto 3

Primero instalamos htpasswd
`sudo apt-get update` y
`sudo apt-get install apache2-utils`

Generamos el hash con usuario: `admin` y contraseña:`admin`
<img width="696" height="65" alt="image" src="https://github.com/user-attachments/assets/80a10b7d-8056-4f16-ae28-2becf9ddbd52" />


Modificamos el `docker-compose.yml` poniendo los labels para Basic Auth y rateLimit. En este caso ponemos `average=5` y `burst=10` para que maneje 5 peticiones por segundo y 10 de golpe.

```
labels:
 - "traefik.http.routers.dashboard.middlewares=dashboard-auth"
 - "traefik.http.middlewares.dashboard-auth.basicauth.users=admin:$$2y$$05$$aLOxQZJZGBiiLK7/WpejpensY8jML0dgAKtMoPt0tfKBBTm7zVEgO"
```

```
labels:
  - "traefik.http.routers.api.middlewares=rate-limit"
  - "traefik.http.middlewares.rate-limit.ratelimit.average=5"
  - "traefik.http.middlewares.rate-limit.ratelimit.burst=10"
```

ANALOGÍA DE LOS MIDDLEWARES:
- Badic Auth: Este middleware sirve en el caso del aeropuerto como una autenticación para el pasajero, por ejemplo, el pasajero debe mostrar sus documentos (cc o pasaporte) además del ticket para poder entrar en el aeropuerto y abordar el avión.

- Rate limitn: En la analogía de el aeropuerto. rate-limit viene funcionando como los controles de seguridad para manejar la concurrencia de los pasajeros, si por ejemplo 20 pasajeros quiern abordar al tiempo hay que poner un limite, por ejemplo de a 5 pasajeros.

# Punto 4
Lo primero es crear una red interna 

En este caso la nombramos como 'airport-network'

y verificamos

<img width="587" height="157" alt="image" src="https://github.com/user-attachments/assets/ec069bd5-3716-4a9f-a009-0a2555032664" />

<img width="929" height="40" alt="image" src="https://github.com/user-attachments/assets/757f159b-754a-4e30-aaa8-9b021ad9e9b8" />


# Pregunta:
- Como nuestro aeropuerto refleja transparencia, escalabilidad y tolerancia a fallos:

 El aeropuerto muestra transparencia al ofrecer una visibilidad completa del flujo de trafico y el estado de los servicios.

Está diseñado para manejar cargas crecientes de pasajeros de manera eficiente y dinámica, adaptándose automáticamente a los cambios en la infraestructura

Contribuye a la tolerancia a fallos al garantizar la alta disponibilidad del sistema y la resiliencia ante fallos en los componentes
 




