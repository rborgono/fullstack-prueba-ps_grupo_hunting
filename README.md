# Prueba Técnica Full Stack - PS Grupo Hunting

Este repositorio/documentación describe el proyecto completo de la aplicación, integrando ambas capas del proyecto (frontend y backend).

---

## Estructura del proyecto

- **Backend:** [Repositorio Backend](https://github.com/rborgono/backend-prueba-ps_grupo_hunting)
- **Frontend:** [Repositorio Frontend](https://github.com/rborgono/frontend-prueba-ps_grupo_hunting)

---

## Cómo levantar la aplicación completa en local

1. Clonar ambos repositorios (frontend y backend).  
2. Levantar el **backend** siguiendo su README.  
3. Levantar el **frontend** siguiendo su README, asegurándose de que la URL del backend esté configurada en `environment.ts`.  
4. La aplicación completa estará disponible en `http://localhost:4200` (o en el puerto correspondiente que se haya escogido para su ejecución).

---

## Lógica y Optimización

A continuación se responderán las preguntas planteadas al final de la prueba.

1. ¿Cómo escalarías esta solución si los datos estuvieran en AWS S3?
RESPUESTA: Mantengo la misma estructura del backend (precisamente por ello se separaron responsabilidades), pero la data en lugar de cargarla directamente desde un archivo local, se cargaría desde una consulta a la BD específica que se encuentre en AWS S3. Y en el archivo de environments se puede configurar las rutas que irán a la BD.

2. ¿Cómo optimizarías el rendimiento si el volumen creciera 100 veces?
RESPUESTA: Si el volumen de datos es alto, lo que se puede hacer es paginar y entregar la solución por página. De este modo no se exige mucho al servidor y como respuesta, viaja menos información de vuelta. Existen otras soluciones para caché de datos, pero ello requiriría investigación de mi parte. Otra solución podría ser tener la data parcializada en más de una BD, pero esta decisión ya pasa por la parte de arquitectura de datos.

3. ¿Cómo asegurarías la API frente a accesos no autorizados?
RESPUESTA: Se pueden utilizar medidas de seguridad como JWT que valide un token de duración limitada que se debe enviar como autenticación en los headers de cada petición. También se puede tener un API Gateway, por ejemplo, si el backend se llevase a AWS, se puede estructurar toda la API en AWS API Gateway otorgándole medidas adicionales de seguridad (número de peticiones, dominios de origen, etc.) que se pueden configurar en dicha herramienta y desde allí que se conecten a funciones AWS Lambda que ejecuten el microservicio de cada endpoint.

---

## Validación de Autoría

La complejidad de esta prueba técnica estaba principalmente en el desarrollo del backend con los endpoints solicitados y en el desarrollo del frontend que hiciera consumo de al menos uno de los endpoints del backend. No se solicitaba una conexión particular a una base de datos o a otra API para traer los datos que se utilizarían para esta prueba.

Dado lo anterior, se decidió construir el backend en Node.js (por la expertiz y experiencia con este entorno), y mantener los datos como harcoding en un archivo ventas.json dentro del directorio data del backend.

El backend fue separado en directorios de controllers, data, helpers y routes, con un archivo principal app.js el cual levanta el servidor al momento de su ejecución. Esta separación permite mantener de modo independiente responsabilidades diferentes dentro del backend teniendo un repositorio más ordenado y legible. La lógica de negocio se concentró en helpers, mientras que en controllers se focalizó el manejo de peticiones y respuestas de la API. En routes se estructuró las rutas de la API (los endpoints).

Por otro lado, el frontend fue desarrollado en Angular (también por la mayor expertiz en este framework). El frontend se estructuró en componentes, modelos y servicios. Se crearon 3 componentes para el aplicativo, uno para las fechas (filtro), otro para el despliegue de la ventas y otro para las ventas por región (en la prueba se pedía consumir al menos un endpoint, se consumieron 2). El service ventas se utiliza para realizar los llamados a la API del backend.

Por simplicidad y por ser una prueba técnica, se estructuró todo en una misma página, con los filtros de fechas arriba y el resultado de las ventas y ventas por región más abajo.

Finalmente, se puede mencionar que no se encontraron durante el desarrollo partes de extrema complejidad en la prueba. Más que la complejidad en sí, tal vez la parte más técnica por el lado del front fue cómo se iba a desplegar la solución y qué herramientas se utilizarían para el despliegue. Se decidió el uso de componentes que es lo que divide una página web en Angular en partes más pequeñas y el uso de comunicación entre componentes para la emisión de eventos desde la interfaz y la renderización de datos en la pantalla. No es algo complejo, pero si no se estructura bien, se puede complicar.

TIEMPO TOTAL UTILIZADO (estimado): 2,5 días.
