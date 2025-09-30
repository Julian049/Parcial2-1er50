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
# Punto 3 - Middlewares

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

