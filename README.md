# 👁 CASSANDRA 👁
Show me the code - Cassandra database

## 1 - RUN DOCKER CONTAINER 
`docker pull cassandra`

`docker container run -d --name cassandra-server cassandra`

`docker exec -it cassandra-server cqlsh`

## 2 - CREATE KEYSPACE
`CREATE KEYSPACE test WITH REPLICATION = { 'class': 'SimpleStrategy', 'replication_factor' : 1};`

`USE test;`

## 3 - CREATE 'CITIES BY COUNTRY' TABLE
TABELA PARA LISTAR DADOS DE TODAS AS CIDADES DE **UM PAÍS**
`CREATE TABLE test.cities_by_country (
   country text, 
   city text, 
   state text,  
   offset text,
   initials text, 
   PRIMARY KEY ((country),city)) 
WITH CLUSTERING ORDER BY (city ASC);`


#### 3.1 - POPULATE TABLE 
`INSERT INTO test.cities_by_country (country, city, state, offset, initials)
VALUES ('Brasil', 'Belo Horizonte', 'Minas Gerais', '-3', 'BH');`

`INSERT INTO test.cities_by_country (country, city, state, offset, initials)
VALUES ('Brasil', 'Contagem', 'Minas Gerais', '-3', 'CN');`

`INSERT INTO test.cities_by_country (country, city, state, offset, initials)
VALUES ('Brasil', 'São Paulo', 'São Paulo', '-3', 'SP');`

`INSERT INTO test.cities_by_country (country, city, state, offset, initials)
VALUES ('EUA', 'São Petersburgo', 'Flórida', '-5', 'SP');`

`INSERT INTO test.cities_by_country (country, city, state, offset, initials)
VALUES ('Russia', 'São Petersburgo', 'Leningrado', '+3', 'SP');`


#### 3.2 - TEST QUERIES 
`SELECT * FROM cities_by_country where city = 'São Petersburgo';` ❌

`SELECT * FROM cities_by_country where initials = 'SP';`  ❌

`SELECT * FROM cities_by_country where country = 'Brasil';` ✔



## 4 - CREATE 'CITIES BY COUNTRY AND COUNTRY' TABLE
TABELA PARA LISTAR DADOS DE TODAS AS CIDADES DE **UM ESTADO**

`CREATE TABLE test.cities_by_country_state (
   country text, 
   state text, 
   city text, 
   foundation text,
   initials text, 
   PRIMARY KEY ((country,state),city)) 
WITH CLUSTERING ORDER BY (city ASC);`

#### 4.1 - POPULATE TABLE 
`INSERT INTO test.cities_by_country_state (country, city, state, offset, initials)
VALUES ('Brasil', 'Belo Horizonte', 'Minas Gerais', '-3', 'BH');`

`INSERT INTO test.cities_by_country_state (country, city, state, offset, initials)
VALUES ('Brasil', 'Contagem', 'Minas Gerais', '-3', 'CN');`

`INSERT INTO test.cities_by_country_state (country, city, state, offset, initials)
VALUES ('Brasil', 'São Paulo', 'São Paulo', '-3', 'SP');`

`INSERT INTO test.cities_by_country_state (country, city, state, offset, initials)
VALUES ('EUA', 'São Petersburgo', 'Flórida', '-5', 'SP');`

`INSERT INTO test.cities_by_country_state (country, city, state, offset, initials)
VALUES ('Russia', 'São Petersburgo', 'Leningrado', '+3', 'SP');`

#### 4.2 - TEST QUERIES 
`SELECT * FROM cities_by_country_state where country = 'Brasil';` ❌

`SELECT * FROM cities_by_country_state where state = 'Minas Gerais';` ❌

`SELECT * FROM cities_by_country_state where country = 'Brasil' and state = 'Minas Gerais';` ✔

`SELECT * FROM cities_by_country_state where country = 'Brasil' and state = 'Minas Gerais' and initials = 'BH';` ❌

`SELECT * FROM cities_by_country_state where country = 'Brasil' and state = 'Minas Gerais' and city = 'Belo Horizonte';` ✔

## 5 - CREATE 'MOVIES_BY_GENRE' TABLE
TABELA PARA LISTAR DADOS DE FILMES DE ACORDO COM O **GENÊRO**

`CREATE TABLE test.movies_by_genre (
   genre text, 
   date date, 
   title text, 
   author text,
   PRIMARY KEY ((genre),date, author, title)) 
WITH CLUSTERING ORDER BY (date DESC,author ASC, title ASC);`

#### 5.1 - POPULATE TABLE 
`INSERT INTO test.movies_by_genre (genre, date, title, author)
VALUES ('Terror', '2022-09-07', 'A morte do cadaver', 'John Doe');`

`INSERT INTO test.movies_by_genre (genre, date, title, author)
VALUES ('Terror', '2022-08-09', 'Bug em Produção', 'Eu mesmo');`

`INSERT INTO test.movies_by_genre (genre, date, title, author)
VALUES ('Terror', '2022-06-10', 'Deploy sexta a noite', 'Todos nós');`

`INSERT INTO test.movies_by_genre (genre, date, title, author)
VALUES ('Ação', '2022-09-07', 'Eliminando Bugs 3', 'John Doe');`

`INSERT INTO test.movies_by_genre (genre, date, title, author)
VALUES ('Terror', '2022-09-07', 'A volta dos que não foram', 'John Travolta');`

#### 5.2 - TEST QUERIES

`SELECT * FROM movies_by_genre where genre = 'Terror';` ✔

`SELECT * FROM movies_by_genre where genre = 'Terror' and date > '2022-06-01' and date < '2022-08-31';` ✔

`SELECT * FROM movies_by_genre where genre = 'Terror' and date > '2022-06-01' and date < '2022-08-31' and author = 'Eu mesmo';` ❌

`SELECT * FROM movies_by_genre where genre = 'Terror' and date = '2022-09-07';` ✔

`SELECT * FROM movies_by_genre where genre = 'Terror' and date = '2022-09-07' and author = 'John Doe';` ✔
