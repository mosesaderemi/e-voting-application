# E-Voting Application

The voter registration module of an electronic voting system, built with Spring Boot. It validates and stores voter details, issues a confirmation token for each new account and removes expired tokens on a schedule.

## Status

In development. The registration and confirmation-token foundation is implemented. Ballot, candidate and result modules are not yet part of this repository.

## Features

- `POST /` endpoint to register a voter with first, middle and last name, email and password
- Bean validation on every field, including an email format check and a password pattern
- Duplicate detection: registration is rejected when the email already exists (case-insensitive)
- Confirmation token entity and service with creation, confirmation and expiry timestamps
- Nightly scheduled job (midnight) that deletes expired tokens
- In-memory H2 database with the H2 console enabled for development

## Tech Stack

| Area | Technology |
| --- | --- |
| Language | Java 17 |
| Framework | Spring Boot 3.0.3 (Web, Data JPA, Validation, HATEOAS, Mail) |
| Database | H2 in-memory |
| Utilities | Lombok |
| Build tool | Maven (wrapper included) |

## Project Structure

```text
e-voting-app/src/main/java/com/evoting/evotingapp
├── controllers          RegistrationController
├── data
│   ├── model            Voter entity
│   └── repositories     VoterRepo
├── exceptions           RegistrationException
├── payload/request      RegistrationRequest, LoginRequest, ConfirmationTokenRequest
├── registration/token   Token entity, services and the expired-token scheduler
├── services             VoterService and implementation
└── EVotingAppApplication.java
```

## Getting Started

### Prerequisites

- JDK 17 or later

### Run

```bash
git clone https://github.com/mosesaderemi/e-voting-application.git
cd e-voting-application/e-voting-app
./mvnw spring-boot:run
```

On Windows use `mvnw.cmd spring-boot:run`. The H2 console is available at `http://localhost:8080/h2-console` with the JDBC URL `jdbc:h2:mem:e_voting_db`.

### Try the registration endpoint

```bash
curl -X POST http://localhost:8080/ \
  -H "Content-Type: application/json" \
  -d '{"first_name":"Ada","middle_name":"Grace","last_name":"Obi","email":"ada@example.com","pass_word":"<password that matches the validation rule>"}'
```

The password rule is defined by the `@Pattern` annotation in `Voter.java`.

## Notes

- Data is stored in memory and is lost when the application stops. Switch the datasource in `application.properties` to MySQL or PostgreSQL for persistence.
- Email delivery is not configured yet. Add `spring.mail.*` properties and a sender component before sending confirmation emails.

## Author

Aderemi Moses Timileyin
GitHub: [@mosesaderemi](https://github.com/mosesaderemi)
LinkedIn: [linkedin.com/in/mosesaderemi-6a70a5405](https://www.linkedin.com/in/mosesaderemi-6a70a5405)

## License

Released under the MIT License. See the `LICENSE` file for details.
