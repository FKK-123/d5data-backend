# Hello World with Spring Security

Spring Boot **3.2.0** API. `GET /hello` returns the plain text `Hello World` and requires a JWT from `POST /login`.

The only user is stored in memory:

| Username | Password |
| --- | --- |
| `test` | `123456` |

The password is checked with BCrypt. A correct login returns a signed JWT. Requests without a valid `Authorization: Bearer` token receive `401`.

## Run

Java 17 is required. Maven is not required globally. `mvnw.cmd` downloads Maven 3.9.9 on the first run.

```text
mvnw.cmd test
mvnw.cmd spring-boot:run
```

## Calls

Login:

```text
curl -s -X POST http://localhost:8080/login -H "Content-Type: application/json" -d "{\"username\":\"test\",\"password\":\"123456\"}"
```

Hello, using the `token` field from the login response:

```text
curl -s http://localhost:8080/hello -H "Authorization: Bearer TOKEN"
```

A wrong password, or `GET /hello` with no token, responds with HTTP 401.

PowerShell:

```text
$login = Invoke-RestMethod -Method Post -Uri http://localhost:8080/login -ContentType application/json -Body '{"username":"test","password":"123456"}'
Invoke-RestMethod -Uri http://localhost:8080/hello -Headers @{ Authorization = "Bearer $($login.token)" }
```

## Tests

`HelloSecurityApplicationTests` covers three cases:

1. `test` / `wrong` is rejected.
2. `test` / `123456` can call `GET /hello` and the body is exactly `Hello World`.
3. `GET /hello` without a token is rejected.

The JWT secret in `application.properties` is a demo value for this exercise.
