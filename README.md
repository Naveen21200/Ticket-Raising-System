# Ticket Raising System (Spring Boot + MySQL + Email OTP)

## Requirements
- Java 17+, Maven 3.8+, MySQL 8+

## Setup
1. Start MySQL. The database `ticket_db` is created automatically (or run `database/schema.sql`).
2. Edit `src/main/resources/application.properties`:
   - `spring.datasource.username` / `spring.datasource.password`
   - `spring.mail.username` / `spring.mail.password` / `app.mail.from`
     (Gmail: enable 2-step verification, create an **App Password**, use it as the password)
3. Run:
   ```
   mvn spring-boot:run
   ```
4. Open http://localhost:8081

## Dev mode
`app.otp.log-to-console=true` prints the OTP in the console, so you can test before SMTP is configured.
Set it to `false` for production.

## API
| Method | Endpoint | Auth | Purpose |
|---|---|---|---|
| POST | /api/auth/send-otp | – | `{email,name}` -> emails OTP |
| POST | /api/auth/verify-otp | – | `{email,otp}` -> `{token}` |
| POST | /api/auth/logout | Bearer | End session |
| POST | /api/tickets | Bearer | Create ticket (sends confirmation email) |
| GET | /api/tickets | Bearer | List my tickets |
| PATCH | /api/tickets/{id}/status | Bearer | Update status |

## Security notes
OTPs are stored SHA-256 hashed, expire in 5 min, allow 5 attempts, and have a 30s resend cooldown.
