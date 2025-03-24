# Despliegue de los servicios vitales para el funcionamiento del proyecto

## preparación
1. abrir docker
2. ejecutar docekr-compose up
3. verificar que los servicios estan corriendo:
  - emqx
  - mongo
  - (El servicio de mongo puede presentar inconvenientes si hace conflicto con un mongoDB local de Windows, evidencia de que eto sucede es cuando se conecta directo desde Compass sin auth e incluso sin levantar el servicio en docker)

## back-end del proyecto:
- desde carpeta APP,
- comando npm run devn 
- target del comando: nodemon api/index.js
(si esto no funciona es porque debe instalarse nodemon, el cual no se instala con las demas dependencias con npm install porque se hizo de forma global: npm install -g nodemon)

### levantar webhooks (end-points):
- los webhooks creados pueden iniciar desconectados sin poderse conectar ni siquiera manualmente
- esto sucede porque como los endpoints se levantan en localhost, la dirección IP en el equipo cambia cada vez
- asegurarse de que la IP actual es la misma que en el docker-compose.ym, en "extra_hosts:"

## front-end del proyecto:
- desde carpeta APP,
- comando npm run dev 
- target del comando: nuxt