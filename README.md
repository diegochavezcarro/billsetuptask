# Ejemplo de Spring Cloud Task y Spring Cloud Data Flow:

### La primera parte (ver mas adelante para lo referido a Spring
Cloud Data Flow) referente a Spring Cloud Task esta basada en:

* https://dataflow.spring.io/docs/batch-developer-guides/batch/spring-task/

### Spring Cloud Task:

* Si se quiere usar mysql setearlo:

docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=task -d mysql:5.7.25

* Primero probar la aplicación con h2. El Test se deberia ejecutar bien.

Si se quieren ver las tablas generadas en H2 tanto por el ejemplo como por 
Spring Cloud Task y descomentar en el pom.xml la parte Web y ver las tablas en:

http://localhost:8080/h2-console

* Si se quiere probar el ejemplo con mysql se puede ejecutar con el spring profile DEV.
En ese caso se usara el application-fev.properties, probar luego si se creó la tabla y su contenido: 

$ docker exec -it mysql bash -l

# mysql -u root -ppassword

mysql> select * from task.BILL_STATEMENTS

Si se quiere ver la ejecucion de la Tarea:

select * from task.TASK_EXECUTION;

### Spring Cloud Data Flow

* Basado en:

https://dataflow.spring.io/docs/batch-developer-guides/batch/data-flow-simple-task/

* En el caso de los ejemplos de Tasks no es necesario levantar Rabbit, si se quiere levantar:

docker run -d --hostname rabbitmq --name rabbitmq -p 15672:15672 -p 5672:5672 rabbitmq:3.7.14-management

* Levantar las diferentes herramientas:

java -jar spring-cloud-dataflow-server-2.4.2.BUILD-20200310.115040-7.jar

java -jar spring-cloud-skipper-server-2.3.2.BUILD-20200310.102318-6.jar

La Shell no es necesaria si se realiza todo desde la consola UI, en el ejemplo a veces sí
la uso:

java -jar spring-cloud-dataflow-shell-2.4.2.BUILD-20200310.115040-7.jar

* Para el ejemplo de Tasks con tareas compuestas, importar tambien las tasks pre armadas:

app import --uri https://dataflow.spring.io/task-maven-latest

* Instalar en Maven el ejemplo, de paso tambien instalar el de

https://github.com/diegochavezcarro/billrun

Los tests andan bien, pero si se quiere acelerar 
con las diferentes pruebas ejecutar en cada uno de ellos:

./mvnw install -DskipTests

* Seguir el tutorial, donde se instala la aplicacion usar:

maven://io.spring:billsetuptask:0.0.1-SNAPSHOT

Existe la posibilidad de usar Docker tambien, ver tutorial, si no coincide version de jdk
utilizar plugin de Spotify con un Dockerfile que incluya version correspondiente o generar
imagen a mano. En el ejemplo esta el plugin y el Dockerfile necesario. Se puede instalar
la imagen local o en Docker Hub.

El SCDF sobrescribe varias de las propiedades de la app. 
Si se quiere sobrescribir alguna de las propiedades que el SCDF usa por defecto
hay que crear un white list:

https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#spring-cloud-dataflow-stream-app-whitelisting

El mismo puede estar en otro jar (ver sección siguiente a la anterior url) llamado de metadata. En el caso de utilizar docker 
de hecho esa metadata va a tener que estar en un jar con la config de maven y no en otra 
imagen de Docker.

Volviendo a remarcar que el SCDF sobrescribe varias de las propiedades de la app, si se
utiliza el perfil dev con la info de mysql el ejemplo no va a andar. El SCDF asi como
viene instalado trae una H2 en memoria, y una de las propiedades que se sobrescriben es
spring.datasource. Si se quieren usar dos bases (una para SCDF) y otra para la base 
aplicativa ver:

https://github.com/spring-cloud/spring-cloud-task/tree/master/spring-cloud-task-samples/multiple-datasources

Tiene que ver tambien con la configuracion por defecto de Spring Boot:

https://medium.com/@joeclever/using-multiple-datasources-with-spring-boot-and-spring-data-6430b00c02e7

* Probar la otra aplicacion:

https://github.com/diegochavezcarro/billrun

Basado en el tutorial:

https://dataflow.spring.io/docs/batch-developer-guides/batch/data-flow-spring-batch/

Cuando se registra la app usar:

maven://io.spring:billrun:0.0.1-SNAPSHOT

* Probar las dos aplicaciones de forma compuesta:

Basado en el tutorial:

https://dataflow.spring.io/docs/batch-developer-guides/batch/data-flow-composed-task/



