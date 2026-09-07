# webAvanzada
Integrantes: 
- Ignacio Reyes
- Patricio Hernandez

Ingeniería Web Avanzada | OII436-1


# Laboratorio 1
# Pregunta 1. ¿Por qué no se recomienda desarrollar directamente sobre main en este laboratorio?

Porque uno de los requisitos mínimos de trabajo colaborativo es que el repositorio principal debe permanecer siempre en un estado ejecutable y libre de errores. Por esta razón debemos desarrollar los cambios en ramas independientes e integrarlos mediante Pull Requests para poder validarlos.

# Pregunta 2. ¿Qué problema se evita al utilizar --skip-git al crear el proyecto Angular?

Evitamos inicializar un segundo repositorio .git anidado. Como ya habíamos inicializado el control de versiones en la raíz del repositorio principal, esto para evitar conflictos de seguimiento de archivos en Git.

# Pregunta 3. ¿Qué verifica npm run build en esta etapa del laboratorio?

Verificamos que nuestra aplicación Angular pueda compilarse correctamente y generar los archivos listos para distribución, asi nos aseguramos de que el proyecto es construible antes de proceder en el pipeline de CI.

# Pregunta 4. ¿Qué utilidad tiene revisar git status o git diff --cache antes de realizar un commit?

Para tener un control exacto sobre lo que vamos a versionar. Ejecutar git status nos permite ver qué archivos hemos modificado, cuáles están preparados y cuáles no están rastreados. Con git diff --cached revisamos exactamente los cambios que ya dejamos preparados para incluir en nuestro próximo commit.

# Pregunta 5. ¿Qué evento activa el workflow ci.yml?

Evento que configuramos para activar el pipeline de CI es la apertura de un Pull Request que apunte hacia nuestra rama main.

# Pregunta 6. En runs-on: ubuntu-latest, ¿qué representa ubuntu-latest?

Representa el entorno virtual administrado por GitHub Actions (con sistema operativo Ubuntu) que se encargará de ejecutar los jobs del pipeline.

# Pregunta 7. Ordene las etapas de validación que ejecuta el job frontend y explique por qué npm ci se ejecuta antes que las pruebas.

El orden de los steps definidos en el pipeline es: 

Obtener código (actions/checkout).

Configurar Node.js (actions/setup-node).

Instalar dependencias (npm ci).

Ejecutar pruebas (npm test).

Construir Angular (npm run build)].

El comando npm ci debe ejecutarse antes que las pruebas porque realiza una instalación limpia ya que sin las dependencias y librerías instaladas el proyecto no puede probarse ni compilarse.

# Pregunta 8. Después del push, indique qué etapa del pipeline falla y qué ocurre con las etapas siguientes.

Falla la etapa de Ejecutar pruebas porque modificamos intencionalmente app.component.spec.ts para que esperara un título incorrecto. Al fallar esta etapa las etapas siguientes se cancelan automáticamente y no se ejecutan marcando todo como fallido.

# Pregunta 9.¿Debería integrarse este Pull Request a main mientras el pipeline está fallando? Justifique.

No, esto propagaría el error al código principal rompiendo el acuerdo de que main debe permanecer siempre ejecutable. El objetivo de CI es detectar estos fallos para que el equipo los corrija antes de hacer el merge.

# Pregunta 10. Clasifique cada elemento como versionable, variable/configuración o secreto/no versionable:

*   package.json: Versionable.
*   API_URL pública: Variable/configuración.
*   AWS_REGION: Variable/configuración.
*   DB_PASSWORD: Secreto/no versionable.
*   API_TOKEN: Secreto/no versionable.
*   terraform.tfstate: Secreto/no versionable

# Pregunta 11. ¿Por qué una contraseña o token no debe escribirse directamente dentro de ci.yml, cd.yml o un archivo TypeScript del frontend?

Porque exponer secretos directamente en el código base o en los flujos de trabajo los hace visibles para cualquier persona que tenga acceso al repositorio, creando un grave riesgo de exposición de credenciales y vulnerando la seguridad de la aplicación. 

# Pregunta 12. Si un secreto real fue incluido en un commit y luego se agrega su archivo a .gitignore, ¿queda solucionado el problema? Explique qué acción adicional debe realizarse.

No queda solucionado porque Git conserva el historial de todos los commits anteriores lo que hace que el secreto siga visible si alguien revisa el historial del repositorio. Lo que hay que hacer es revocar ese secreto para que deje de funcionar y reescribir el historial de Git para eliminar su rastro.

# Pregunta 13. ¿Qué diferencia existe entre terraform validate, terraform plan y terraform apply?

terraform validate: Verifica la sintaxis y estructura del archivo de configuración sean correctas.

terraform plan: Genera un plan de ejecución y muestra qué cambios se van a realizar en la infraestructura sin aplicar nada todavía.

terraform apply: Aplica y ejecuta efectivamente los cambios sobre el entorno.

# Pregunta 14. ¿Por qué ci.yml se activa con pull_request y cd.yml se activa con push sobre main?

El pipeline ci.yml se ejecuta en el pull_request porque su función es validar los cambios en un entorno aislado para proteger la rama principal de posibles errores antes de aprobar el código. 

El pipeline cd.yml se activa con un push sobre main porque cuando el código ya fue validado y fusionado en la rama principal el siguiente paso es desplegar o entregar automáticamente esa versión hacia el ambiente de staging.

# Pregunta 15. ¿Qué función cumple Terraform dentro de este flujo de CD?

Terraform cumple la función de preparar la infraestructura como código, Terraform automatiza la preparación del entorno de staging local (creando carpetas y moviendo los archivos compilados del frontend).

# Pregunta 16. ¿Por qué el workflow usa {{ secrets.DEMO_TOKEN }} en lugar de escribir el valor directamente?

Para inyectar la credencial de forma segura únicamente durante el tiempo de ejecución runtime del pipeline desde las variables almacenadas y encriptadas en GitHub Secrets, esto respetando el no alojar secretos expuestos dentro del código o archivos en el repositorio.
