# Spring_Boot_Kotlin

API dos Vingadores - LiveCode DIO - 01/04/2021
Desenvolvimento de uma API utilizando SpringBoot + Kotlin com o intuito de cadastro de Vingadores.

Tecnologias / Frameworks / IDE
Intelij
SpringBoot2.4.4
Maven
Kotlin
SpringData JPA
PostgreSQL
Rota de voo
Java 8
Heroku
Criação do esqueleto do projeto
https://start.spring.io/
API do contrato
https://editor.swagger.io/

Recursoavenger

OBTER - 200 OK

GET {id}/detail - 200 OK ou 404 Não encontrado

POST - 201 Criado ou 400 Bad Request

PUT {id} - 202 Aceito ou 404 Não Encontrado

DELETE {id} - 202 Aceito ou 404 Não Encontrado

{
  "nick": "spider-man",
  "person": "Peter Parker",
  "description": "sobre poderes",
  "history": "a história"
}
Design Arquitetônico
Camada de Aplicação (controladores, configurações, handle de exceção, dtos de request e resposta, validações de bean)
Camada de Domínio (modelo avenger, interface repositório, serviço)
Camada de Infraestrutura (repositório jpa, entidade avenger, proxy implementa repositório de interface e utiliza o repositório jpa para comunicação com banco de dados)
Testes
Flyway (banco de dados/migração)
create table avenger (
    id bigserial not null,
    nick varchar(36),
    person varchar(128),
    description varchar(128),
    history text
    primary key (id)
);

alter table avenger add constraint UK_5r88eemotwgru6k0ilqb2ledh unique (nick);
Perfis
aplicativo.yaml
aplicativo-dev.yaml
aplicativo-heroku.yaml
spring:
  application:
    name: avengers
  config:
    # This configuration allow use profiles as spring 2.3.x version
    # In spring 2.4.x version, has changed to:
    # spring:
    #  profiles:
    #    group:
    #      <group>: dev, auth
    use-legacy-processing: true
  profiles:
    active: dev
  jmx:
    enabled: false
  data:
    jpa:
      repositories:
        bootstrap-mode: deferred
  jpa:
    open-in-view: false
    properties:
      hibernate.jdbc.time_zone: UTC
      hibernate.generate_statistics: false
      hibernate.jdbc.batch_size: 25
      hibernate.order_inserts: true
      hibernate.order_updates: true
      hibernate.query.fail_on_pagination_over_collection_fetch: true
      hibernate.query.in_clause_parameter_padding: true
    hibernate:
      ddl-auto: none
      naming:
        physical-strategy: org.springframework.boot.orm.jpa.hibernate.SpringPhysicalNamingStrategy
        implicit-strategy: org.springframework.boot.orm.jpa.hibernate.SpringImplicitNamingStrategy
  main:
    allow-bean-definition-overriding: true
  task:
    execution:
      thread-name-prefix: avengers-task-
      pool:
        core-size: 2
        max-size: 50
        queue-capacity: 10000
    scheduling:
      thread-name-prefix: avengers-scheduling-
      pool:
        size: 2
  output:
    ansi:
      console-available: true

server:
  port: 9090
  servlet:
    session:
      cookie:
        http-only: true
    context-path: /avengers
spring:
  profiles:
    active: dev
  jackson:
    serialization:
      indent-output: true
  datasource:
    type: com.zaxxer.hikari.HikariDataSource
    url: jdbc:postgresql://localhost:25433/${DB_NAME}
    username: ${DB_USER}
    password: ${DB_PASSWORD}
  jpa:
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    show-sql: true
spring:
  profiles:
    active: heroku
  jackson:
    serialization:
      indent-output: true
  datasource:
    type: com.zaxxer.hikari.HikariDataSource
    url: jdbc:postgresql://${HOST}/${DB_NAME}
    username: ${DB_USER}
    password: ${DB_PASSWORD}
  jpa:
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    show-sql: false
Dcoker
Configuração do ambiente
DB_USER=dio.avenger
DB_PASSWORD=dio.avenger
DB_NAME=avengers
YAML (backend-services.yaml)
version: '3.2'
services:
  postgres:
    image: postgres:12-alpine
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      ALLOW_IP_RANGE: 0.0.0.0/0
    ports:
      - "25433:5432"
    volumes:
      - pdb12:/var/lib/postgresql/data
    networks:
      - postgres-compose-network

  teste-pgadmin-compose:
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: "avengers@email.com"
      PGADMIN_DEFAULT_PASSWORD: "123456"
    ports:
      - "5556:80"
    depends_on:
      - postgres
    networks:
      - postgres-compose-network

volumes:
  pdb12:
networks:
  postgres-compose-network:
    driver: bridge
Script / Comandos
docker-compose -f backend-services.yaml up -d(implantar) / docker-compose -f backend-services.yaml down(desimplantar)

Iniciar API

./mvnw spring-boot:run -Dspring-boot.run.profiles=dev -Dspring-boot.run.jvmArguments="-Xmx256m -Xms128m" -Dspring-boot.run.arguments="'--DB_USER=dio.avenger' '--DB_PASSWORD=dio.avenger' '--DB_NAME=avengers'"
Heroku
Criar aplicativo
Linkar com Github
Definir variáveis ​​de ambiente
Perfil
web: java -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap $JAVA_OPTS -Dserver.port=$PORT -Dspring.profiles.active=heroku -jar target/*.jar
Sobre
dio-avengers-api

Recursos
 Leia-me
 Atividade
 Propriedades personalizadas
Estrelas
 16 estrelas
Observadores
 3 assistindo
Garfos
 6 garfos
Repositório de relatórios
Lançamentos
Nenhum lançamento publicado
Pacotes
Nenhum pacote publicado
Implantações
1
 dio-avengers-api inativo
línguas
Kotlin
94,9%
 
Concha
5,1%
Rodapé
© 2024 GitHub, Inc.
Navegação no rodapé
Termos
Privacidade
Segurança
Status
Documentos
Contato
Gerenciar cookies
Não compartilhe minhas informações pessoais