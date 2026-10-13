# test-manager-library-3145556-infra

> **Prueba técnica · Ficha ADSO 3145556**
> Entrega: **martes 13 de octubre de 2026, a las 5:00 p. m.** (17:00, hora Colombia).

Este repositorio es la **infraestructura** de *Manager Library*: contenedores, red, volúmenes y el motor de base de datos vacío. **Empieza vacío a propósito**: se construye en el fork de cada aprendiz.

## Instrucciones

Cada aprendiz debe **crear el fork** de los seis repositorios del proyecto y **resolver el proyecto
con el spec planteado**.

1. Hacer fork, a su cuenta de GitHub, de cada repositorio de la tabla del final.
2. Leer el spec en [`test-manager-library-3145556-docs`](https://github.com/code-sena/test-manager-library-3145556-docs).
   Se entrega en dos versiones: `spec-python/` y `spec-.net/`.
3. Desarrollar en los forks.

## El reto se desarrolla con React y PHP (Laravel)

El spec está escrito para Python y para .NET, pero el reto **no** se hace en esos lenguajes:

| Capa | Tecnología del reto |
|---|---|
| Frontend | React |
| Backend | PHP con Laravel |

Lo que el spec define sobre el negocio —historias, criterios de aceptación, reglas, contrato de la
API, modelo de datos— se respeta. Lo que define sobre la tecnología se traduce a React y Laravel.

## La prueba no consiste en escribir el código

El propósito principal es ver la **capacidad de desempeño con SDD** (*Spec-Driven Development*,
desarrollo guiado por especificación): cómo se lee, se interpreta y se aplica una especificación
para llevarla a un stack distinto. El código es el medio, no el fin.

## Entrega

Cuenta lo que quede **subido a los forks antes de las 5:00 p. m.** (17:00). Lo que llegue después no se revisa.
No hay prórroga: suban avances parciales con commits pequeños durante la prueba.

## Los seis repositorios

| Repositorio | Qué va ahí |
|---|---|
| [`test-manager-library-3145556-docs`](https://github.com/code-sena/test-manager-library-3145556-docs) | El spec: `spec-python/` y `spec-.net/` |
| [`test-manager-library-3145556-api`](https://github.com/code-sena/test-manager-library-3145556-api) | Backend en PHP (Laravel) |
| [`test-manager-library-3145556-app`](https://github.com/code-sena/test-manager-library-3145556-app) | Frontend en React |
| [`test-manager-library-3145556-page`](https://github.com/code-sena/test-manager-library-3145556-page) | Sitio público estático de presentación |
| [`test-manager-library-3145556-infra`](https://github.com/code-sena/test-manager-library-3145556-infra) | Contenedores, red, volúmenes y motor de base de datos vacío |
| [`test-manager-library-3145556-tool`](https://github.com/code-sena/test-manager-library-3145556-tool) | Utilidades: sembrador de datos de demostración |

## Cómo levantar

```bash
cp .env.example .env        # completar los valores reales; .env nunca se versiona
docker compose up -d        # motor vacío + api (migra y crea el admin al arrancar) + app
```

Los seis repositorios deben estar clonados como carpetas hermanas (`../api`, `../app`).

| Servicio | Puerto | Qué es |
|---|---|---|
| `db` | 5432 | PostgreSQL 16, base `library`, **sin tablas** |
| `api` | 8000 | Backend Laravel (`/api/v1`) |
| `app` | 5173 | Frontend React |

### Qué hace y qué no hace este repositorio

- Crea el motor vacío, el usuario, la red y los volúmenes. **No contiene `CREATE TABLE`** ([ADR-001](https://github.com/merari0322/test-manager-library-3145556-docs/blob/main/spec-python/adr/adr-001-propiedad-del-esquema.md)):
  el esquema `library`, las tablas, restricciones y la semilla de categorías los crean las migraciones de `api`.
- Supuesto declarado (ver INC-10 e INC-11 en ADR-005 del repo `docs`): ADR-001 se lee como una regla sobre la base de datos,
  por eso este `docker-compose` también levanta `api` y `app`, como pide T-20.

### Evidencia (T-20)

```bash
docker compose exec db psql -U library -d library -c "\dt library.*"
# Did not find any relation named "library.*"  → el motor arranca vacío
```
