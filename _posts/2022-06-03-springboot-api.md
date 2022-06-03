---
title: Spring Boot API
author: mario
date: 2022-06-03 16:15:00 +0800
categories: [Java, API, Springboot]
tags: [java, springboot, json, api]
render_with_liquid: false
---

Spring Boot API
=======================

Spring Boot web services using JPA

[https://github.com/gerardfp/demo](https://github.com/gerardfp/demo)

Database server
---------------

### Màquina virtual

Descarrega aquesta imatge de disc dur virtual: [debian-11-nocloud-amd64.qcow2](https://github.com/gerardfp/gerardfp.github.io/releases/download/debian-11-nocloud-amd64.qcow2)

Crea una nova màquina virtual amb la imatge descarregada.

Inicia la màquina virtual i esbrina la seva IP.

Descarrega aquesta clau privada: [id_rsa](https://github.com/gerardfp/gerardfp.github.io/releases/download/id_rsa)

Estableix permisos ``600`` al fitxer de la clau:

```bash
chmod 600 /path/to/id_rsa
```

Utilitza aquesta clau per a accedir a la màquina virtual per ssh:

```bash
ssh -i /path/to/id_rsa root@192.168.122.1
```

### Docker Postgresql

Instal·la Docker a la màquina virtual:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
```

Inicia un contenidor Docker amb la base de dades PostreSQL:

```bash
docker run -d --name myapi-postgres -e POSTGRES\_PASSWORD=mysecretpassword -e POSTGRES\_USER=myapidbadmin -e POSTGRES\_DB=myapidb -p 5432:5432 -v /root/db\_data:/var/lib/postgresql/data postgres
```

Habilita l'inici automàtic del contenidor:

`/etc/systemd/system/myapi-postgres.service`

```bash
[Unit]
Description=MyApi container
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker start -a myapi-postgres
ExecStop=/usr/bin/docker stop -t 2 myapi-postgres

[Install]
WantedBy=default.target
```

```bash
systemctl enable myapi-postgres.service
```

El següent pas és accedir al contenidor per a crear l'esquema de la base de dades.

Esbrina el ``CONTAINER ID`` amb la comanda:

```bash
docker ps
```

Inicia un shell (_bash_) al contenidor amb la comanda:

```bash
docker exec -it 6516a4b4e3c2 bash
```

Accedeix al shell psql amb la comanda:

```bash
psql -U myapidbadmin myapidb
```

Copia i enganxa aquest script SQL al shell:

```sql
DROP TABLE IF EXISTS movie, actor, genre, movie_actor, movie_genre CASCADE;

CREATE TABLE IF NOT EXISTS movie (
	movieid uuid NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
	title text,
	imageurl text);

CREATE TABLE IF NOT EXISTS actor (
	actorid uuid NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
	name text,
	imageurl text);

CREATE TABLE IF NOT EXISTS genre (
	genreid uuid NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
	label text);

CREATE TABLE IF NOT EXISTS movie_actor (
	movieid uuid REFERENCES movie(movieid) ON DELETE CASCADE,
	actorid uuid REFERENCES actor(actorid) ON DELETE CASCADE);

CREATE TABLE IF NOT EXISTS movie_genre (
	movieid uuid REFERENCES movie(movieid) ON DELETE CASCADE,
	genreid uuid REFERENCES genre(genreid) ON DELETE CASCADE);

INSERT INTO movie(title, imageurl) VALUES
	('Movie One','movie1.jpg'),
	('Movie Two','movie2.jpg'),
	('Movie Three','movie3.jpg'),
	('Movie Four','movie4.jpg');

INSERT INTO actor(name, imageurl) VALUES
	('Actor One','actor1.jpg'),
	('Actor Two','actor2.jpg'),
	('Actor Three','actor3.jpg'),
	('Actor Four','actor4.jpg'),
	('Actor Five','actor5.jpg');

INSERT INTO genre(label) VALUES
	('Genre One'),
	('Genre Two'),
	('Genre Three');

INSERT INTO movie_actor VALUES
	((SELECT movieid FROM movie WHERE title='Movie One'),(SELECT actorid FROM actor WHERE name='Actor One')),
	((SELECT movieid FROM movie WHERE title='Movie One'),(SELECT actorid FROM actor WHERE name='Actor Two')),
	((SELECT movieid FROM movie WHERE title='Movie Two'),(SELECT actorid FROM actor WHERE name='Actor Three')),
	((SELECT movieid FROM movie WHERE title='Movie Two'),(SELECT actorid FROM actor WHERE name='Actor Four')),
	((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT actorid FROM actor WHERE name='Actor Four')),
	((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT actorid FROM actor WHERE name='Actor Five')),
	((SELECT movieid FROM movie WHERE title='Movie Four'),(SELECT actorid FROM actor WHERE name='Actor One')),
	((SELECT movieid FROM movie WHERE title='Movie Four'),(SELECT actorid FROM actor WHERE name='Actor Four'));

INSERT INTO movie_genre VALUES
    ((SELECT movieid FROM movie WHERE title='Movie One'),(SELECT genreid FROM genre WHERE label='Genre One')),
    ((SELECT movieid FROM movie WHERE title='Movie One'),(SELECT genreid FROM genre WHERE label='Genre Two')),
    ((SELECT movieid FROM movie WHERE title='Movie Two'),(SELECT genreid FROM genre WHERE label='Genre One')),
    ((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT genreid FROM genre WHERE label='Genre One')),
    ((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT genreid FROM genre WHERE label='Genre Two')),
    ((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT genreid FROM genre WHERE label='Genre Three'));
```

Spring boot
-----------

Accedeix a [spring initializr](https://start.spring.io/) per a generar un projecte **Spring Boot**

Selecciona `Gradle Project`

Afegeix les dependències:

* Spring Web
* Spring Data JPA
* PostgreSQL Driver

Genera el projecte i descomprimeix-lo. Obre'l amb IntelliJ

Configura l'accés a la base de dades:

`src/main/resources/application.properties`

```bash
spring.datasource.url= jdbc:postgresql://192.168.122.99:5432/myapidb
spring.datasource.username= myapidbadmin
spring.datasource.password= mysecretpassword
spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation= true
spring.jpa.properties.hibernate.dialect= org.hibernate.dialect.PostgreSQLDialect
```

L'arquitectura bàsica de la nostra ApiHttp amb Spring Boot serà aquesta:

![](https://i.imgur.com/NozvyLb.png)

### Model

Les classes Model serveixen per a crear objectes amb les dades i així poder transportar-les d'un component a un altre.

Començarem creant la classe ``Movie`` que ens servirà per a transportar les dades d'una pel·lícula:

src/main/java/com/example/demo/domain/model/Movie.java 

```java
import javax.persistence.*;
import java.util.UUID;

@Entity
@Table(name = "movie")
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    public UUID movieid;

    public String title;
    public String imageurl;
}
```

### Repository

src/main/java/com/example/demo/repository/MovieRepository.java

```java
import com.example.demo.domain.model.Movie;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.UUID;

public interface MovieRepository extends JpaRepository<Movie, UUID> {

}
```

### Controller

src/main/java/com/example/demo/controller/MovieController.java

```java
import com.example.demo.domain.model.Movie;
import com.example.demo.repository.MovieRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/movies")
public class MovieController {

    @Autowired
    private MovieRepository movieRepository;

    @GetMapping("/")
    public List<Movie> findAllMovies() {
        return movieRepository.findAll();
    }

    @PostMapping("/")
    public Movie createMovie(@RequestBody Movie movie) {
        return movieRepository.save(movie);
    }
}
```

Database migrations
-------------------

La llibreria [Flyway](https://flywaydb.org/) permet gestionar els canvis en l'esquema de la base de dades.

Agegeix Flyway al projecte:

`build.gradle`

```java
plugins {
    ...

    id 'org.flywaydb.flyway' version '8.0.2'

}


flyway {
    configFiles = ['src/main/resources/application.properties']
}


dependencies {
    ...

    implementation 'org.flywaydb:flyway-core:8.0.1'

}
```

Configura els paràmetres d'accés a la base de dades:

`src/main/resources/application.properties`

```java
flyway.url=jdbc:postgresql://192.168.122.99:5432/myapidb
flyway.schemas=public
flyway.user=myapidbadmin
flyway.password=mysecretpassword
spring.flyway.baseline_on_migrate=true
```

Crea una primera versió de l'esquema de la base de dades. Les migracions de l'esquema de la base de dades es defineixen creant arxius al directori `resources/db/migration/`. El nom d'aquests arxius ha de seguir una nomenclatura específica (veure: [migrations#naming](https://flywaydb.org/documentation/concepts/migrations#naming-1))

Crea l'arxiu `resources/db/migration/V1__createdatabase.sql`:

`resources/db/migration/V1__createdatabase.sql`

```sql
CREATE TABLE IF NOT EXISTS movie (
    movieid uuid NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
    title text,
    synopsis text,
    imageurl text);

CREATE TABLE IF NOT EXISTS actor (
    actorid uuid NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
    name text,
    imageurl text);

CREATE TABLE IF NOT EXISTS genre (
    genreid uuid NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
    label text);

CREATE TABLE IF NOT EXISTS movie_actor (
    movieid uuid REFERENCES movie(movieid) ON DELETE CASCADE,
    actorid uuid REFERENCES actor(actorid) ON DELETE CASCADE,
    PRIMARY KEY (movieid, actorid));

CREATE TABLE IF NOT EXISTS movie_genre (
    movieid uuid REFERENCES movie(movieid) ON DELETE CASCADE,
    genreid uuid REFERENCES genre(genreid) ON DELETE CASCADE,
    PRIMARY KEY (movieid, genreid));
;

INSERT INTO movie(title, synopsis, imageurl) VALUES
    ('Movie One','This is the One Movie','movie1.jpg'),
    ('Movie Two','The Two Movie is the next','movie2.jpg'),
    ('Movie Three','The Trilogy','movie3.jpg'),
    ('Movie Four','Four movies is too much','movie4.jpg');

INSERT INTO actor(name, imageurl) VALUES
    ('Actor One','actor1.jpg'),
    ('Actor Two','actor2.jpg'),
    ('Actor Three','actor3.jpg'),
    ('Actor Four','actor4.jpg'),
    ('Actor Five','actor5.jpg');

INSERT INTO genre(label) VALUES
    ('Genre One'),
    ('Genre Two'),
    ('Genre Three');

INSERT INTO movie_actor VALUES
    ((SELECT movieid FROM movie WHERE title='Movie One'),(SELECT actorid FROM actor WHERE name='Actor One')),
    ((SELECT movieid FROM movie WHERE title='Movie One'),(SELECT actorid FROM actor WHERE name='Actor Two')),
    ((SELECT movieid FROM movie WHERE title='Movie Two'),(SELECT actorid FROM actor WHERE name='Actor Three')),
    ((SELECT movieid FROM movie WHERE title='Movie Two'),(SELECT actorid FROM actor WHERE name='Actor Four')),
    ((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT actorid FROM actor WHERE name='Actor Four')),
    ((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT actorid FROM actor WHERE name='Actor Five')),
    ((SELECT movieid FROM movie WHERE title='Movie Four'),(SELECT actorid FROM actor WHERE name='Actor One')),
    ((SELECT movieid FROM movie WHERE title='Movie Four'),(SELECT actorid FROM actor WHERE name='Actor Four'));

INSERT INTO movie_genre VALUES
    ((SELECT movieid FROM movie WHERE title='Movie One'),(SELECT genreid FROM genre WHERE label='Genre One')),
    ((SELECT movieid FROM movie WHERE title='Movie One'),(SELECT genreid FROM genre WHERE label='Genre Two')),
    ((SELECT movieid FROM movie WHERE title='Movie Two'),(SELECT genreid FROM genre WHERE label='Genre One')),
    ((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT genreid FROM genre WHERE label='Genre One')),
    ((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT genreid FROM genre WHERE label='Genre Two')),
    ((SELECT movieid FROM movie WHERE title='Movie Three'),(SELECT genreid FROM genre WHERE label='Genre Three'));
```

Heroku deploy
-------------

* Afegeix aquest arxiu de configuració del projecte:
    
    `src/main/resources/application-production.properties`

    ```bash
    spring.datasource.url=${JDBC_DATABASE_URL:}
    spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect

    server.port=${PORT:}
    ```
    
    Les variables ${JDBC\_DATABASE\_URL:} i ${PORT:} són variables d'entorn proporcionades per Heroku.
    
* Afegeix aquest arxiu al directori arrel del projecte. És la comanda que executarà Heroku per a iniciar l'aplicació:
    
    `Procfile` 
    
    ```bash
    web: java -Dspring.profiles.active=production -jar build/libs/myapi-0.0.1-SNAPSHOT.jar
    ```
    
    Amb l'opció -Dspring.profiles.active=production li diem que agafi l'arxiu de configuració application-**production**.properties
    
    myapi-0.0.1-SNAPSHOT: El nom del projecte el trobaràs a l'arxiu settings.gradle, i la versió a l'arxiu build.gradle.
    
* Publica el projecte a GitHub.
    
* Crea un compte a [Heroku](heroku.com/)
    
* Crea una nova app a Heroku:
    
    ![](https://i.imgur.com/2gfvXQ0.png)
    * Escull "GitHub" com a Deployment method
        
        ![](https://i.imgur.com/lqSeE8t.png)
    * Connecta el repositori GitHub
        
    * Activa el desplegament automàtic (cada cop que facis un push, es desplegarà de nou la app)
        
        ![](https://i.imgur.com/EJdbBlO.png)
    * Per últim fes el desplegament inicial de la app
        
        ![](https://i.imgur.com/dVAwuVe.png)
    * Afegeix el servidor Postgres a Heroku:
        
        Ves a la pestanya 
        
        ![](https://i.imgur.com/5VjS43c.png) 
        
        i afegeix l'add-on "Heroku postgres"
        
        ![](https://i.imgur.com/xoA4vJW.png)

File uploads
------------

Afegirem una migració de la base de dades per a crear una taula que emmagatzemi els arxius que carregin (_upload_)

Crea aquest arxiu de migració:

`src/main/resources/db/migration/V2__filetable.sql`

```sql
CREATE TABLE file (
    fileid UUID NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
    contenttype TEXT,
    data bytea);
```

Creem el model:

`src/main/java/com/example/demo/domain/model/File.java` 

```java
import org.hibernate.annotations.Type;

import javax.persistence.*;
import java.util.UUID;

@Entity
@Table(name = "file")
public class File {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    public UUID fileid;

    public String contenttype;

    @Lob
    @Type(type="org.hibernate.type.BinaryType")
    public byte[] data;
}
```

Creem el repository:

`src/main/java/com/example/demo/repository/FileRepository.java`

```java
import com.example.demo.domain.model.File;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.UUID;

public interface FileRepository extends JpaRepository<File, UUID> {

}
```

I per últim el controlador:

`src/main/java/com/example/demo/controller/FileController.java`

```java
import com.example.demo.domain.model.File;
import com.example.demo.repository.FileRepository;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import java.util.List;
import java.util.UUID;

@RestController
@RequestMapping("/files")
public class FileController {

    private final FileRepository fileRepository;
    FileController(FileRepository fileRepository){
        this.fileRepository = fileRepository;
    }

    @PostMapping
    public String upload(@RequestParam("file") MultipartFile uploadedFile) {
        try {
            File file = new File();
            file.contenttype = uploadedFile.getContentType();
            file.data = uploadedFile.getBytes();

            return fileRepository.save(file).fileid.toString();
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    @GetMapping("/{id}")
    public ResponseEntity<byte[]> getFile(@PathVariable UUID id) {
        File file = fileRepository.findById(id).orElse(null);

        if (file == null) return ResponseEntity.notFound().build();

        return ResponseEntity.ok()
                .contentType(MediaType.valueOf(file.contenttype))
                .contentLength(file.data.length)
                .body(file.data);
    }
}
```

Autenticació i autorització
---------------------------

Crea aquest arxiu de migració:

`src/main/resources/db/migration/V3__usertable.sql` 

```sql
CREATE TABLE usser (
    userid uuid NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
    username varchar(24) NOT NULL UNIQUE,
    password varchar(255) NOT NULL,
    role varchar(10),
    enabled boolean DEFAULT true
  );



  -- afegim un usuari de prova
  CREATE EXTENSION IF NOT EXISTS pgcrypto;
  INSERT INTO usser (username, password) VALUES ('user', crypt('pass', gen_salt('bf')));
```

Afegeix la llibreria `spring-boot-starter-security`:


`build.gradle`

```java
dependencies {
    ...

    implementation 'org.springframework.boot:spring-boot-starter-security'

}
```

Creem el model `User`:


`src/main/java/com/example/demo/domain/model/User.java`

```java
import javax.persistence.*;
import java.util.UUID;

@Entity
@Table(name="usser")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    public UUID userid;

    public String username;
    public String password;
    public String role;
    public boolean enabled;

}
```

Creem el repository:

`src/main/java/com/example/demo/repository/UserRepository.java` 

```java
import com.example.demo.domain.model.User;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.UUID;

public interface UserRepository extends JpaRepository {

    User findByUsername(String username);
}
```

Afegim la configuració de seguretat:

`src/main/java/com/example/demo/SecurityConfig.java`

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

import javax.sql.DataSource;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired private DataSource dataSource;

    @Bean
    public BCryptPasswordEncoder getPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity httpSecurity) throws Exception {
        httpSecurity
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and().csrf().disable()
                .authorizeRequests()
                    //.mvcMatchers("/users/register/")
                    //    .permitAll()
                    .anyRequest()
                        .authenticated()
                .and()
                .httpBasic();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth
                .jdbcAuthentication()
                .dataSource(dataSource)
                .usersByUsernameQuery("select username, password, enabled from usser where username = ?")
                .authoritiesByUsernameQuery("select username, role from usser where username = ?")
                .passwordEncoder(getPasswordEncoder());
    }
}
```

### Registre d'usuaris

Per al registre d'usuaris necessitarem una classe (DTO) que per guardar les dades que ens envia l'usuari (username i password):

`src/main/java/com/example/demo/domain/dto/UserRegisterRequest.java`

```java
public class UserRegisterRequest {
    public String username;
    public String password;
}
```

També necessitarem un controlador per atendre les peticions de registre:

`src/main/java/com/example/demo/controller/UserController.java`

```java
import com.example.demo.domain.dto.UserRegisterRequest;
import com.example.demo.domain.model.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired private UserRepository userRepository;
    @Autowired private BCryptPasswordEncoder passwordEncoder;

    @PostMapping("/register")
    public String register(@RequestBody UserRegisterRequest userRegisterRequest) {

        if (userRepository.findByUsername(userRegisterRequest.username) == null) {
            User user = new User();
            user.username = userRegisterRequest.username;
            user.password = passwordEncoder.encode(userRegisterRequest.password);
            user.enabled = true;
            userRepository.save(user);
            return "OK";   // TODO
        }
        return "ERROR";    // TODO
    }
}
```

Per últim caldrà permetre l'accés a usuaris no autenticats a l'endpoint de registre (/users/register):

src/main/java/com/example/demo/SecurityConfig.java .mvcMatchers("/users/register/") .permitAll()

ResponseEntity
--------------

Per a que els mètodes REST retornin respostes de forma adequada podem utilitzar la classe ``ResponseEntity``.

Aquesta classe té uns mètodes _builder_ que ens permeten establir el [HttpStatus](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes), les capçaleres HTTP i el cos de la resposta.

Haurem d'implementar els mètdoes _Mapping_ dels controlladors de forma que retornin un objecte de classe ``ResponseEntity<?>``

Per exemple, en el següent codi retornem l'status 200 (OK) i afegim al cos de la resposta l'objecte ``movie`` que tot just s'ha creat.

```java
@PostMapping
public ResponseEntity<?> createMovie(@RequestBody Movie movie, Authentication authentication) {
    Movie movie = movieRepository.save(movie);
    return ResponseEntity.ok().body(movie);
}
```

L'objecte ``movie`` que hem posat al ``body()`` es serialitzarà a dades JSON així:

```json
{
    "movieid": "c4806e2b-2e19-4e32-a7cd-8ead8b32350e",
    "title": "Movie Title",
    "imageurl": "/url/to/image"
}
```

Projections
-----------

Una projecció és quan al "select" d'una consulta posem només un subconjunt de camps.

Per a fer-ho amb un JpaRepository primer definirem en un ``interface`` quins són els camps que volem seleccionar:

```java
public interface ProjectionMovie {
    UUID getMovieid();
    String getTitle();

    Set<ProjectionActor> getActors();
}
```

Veiem que en lloc de definir els camps, hem de definir _getters_ seguint l'estàndard [JavaBeans](https://docs.oracle.com/javase/tutorial/javabeans/writing/properties.html).

Després al Repository podem fer que les consultes retornin objectes conforme a aquests interfaces:

```java
public interface MovieRepository extends JpaRepository<Movie, UUID> {

    List<ProjectionMovie> findBy();
}
```

Açò funcionarà per a les consultes que JPA derivades del nom: [Query Methods](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.query-creation)

Relacions
---------

### @ManyToMany

En una relació ManyToMany entre dues entitats hem d'escollir primer una de les dos entitats com la "propietària" de la relació i l'altra com a "no-propietària".

A l'entitat "pripietària" definirem les anotacions ``@ManyToMany`` i ``@JoinTable``:

```java
@Entity
@Table(name = "movie")
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    public UUID movieid;

    public String title;
    public String imageurl;

        @ManyToMany
    @JoinTable(name = "movie_actor", joinColumns = @JoinColumn(name = "movieid"), inverseJoinColumns = @JoinColumn(name = "actorid"))
    public Set<Actor> actors;
    
}
```

A l'entitat "no-propietària" definirem l'anotació ``@ManyToMany`` fent referència al camp de l'entitat "propietària" que defineix la relació:

```java
@Entity
@Table(name = "actor")
public class Actor {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    public UUID actorid;

    public String name;
    public String imageurl;

        @ManyToMany(mappedBy = "actors")
    public Set<Movie> movies;
    
}
```

Com la relació és bidireccional, quan la llibreria Jackson faci la serialització a JSON, es produeix una dependència circular (recursió infinita). Podem tallar aquesta recursió amb l'anotació ``@JsonIgnoreProperties``:

```java
@Entity
@Table(name = "movie")
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    public UUID movieid;

    public String title;
    public String imageurl;

    @ManyToMany
    @JoinTable(name = "movie_actor", joinColumns = @JoinColumn(name = "movieid"), inverseJoinColumns = @JoinColumn(name = "actorid"))
        @JsonIgnoreProperties("movies")
    
    public Set<Actor> actors;
}

@Entity
@Table(name = "actor")
public class Actor {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    public UUID actorid;

    public String name;
    public String imageurl;

    @ManyToMany(mappedBy = "actors")
        @JsonIgnoreProperties("actors")
    
    public Set<Movie> movies;
}
```

-------------

Webgrafía:

- [gerardfp.github.io](https://gerardfp.github.io/)