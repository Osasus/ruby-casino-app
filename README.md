🎰 Ruby Casino Slot Demo

A full-stack, pretend casino slot machine web application built with Ruby (Sinatra) and an SQLite backend. This demo features user authentication, a simulated economy, participate-to-win jackpots, and an admin dashboard where the slot odds can be changed in real-time.

    Disclaimer: This is a simulated demo application meant for educational and portfolio purposes. No real money is used or can be deposited.

✨ Features
👤 User Features

    Authentication: Secure login and registration using bcrypt. Includes brute-force protection (locks out after 3 failed attempts within 60 seconds).

    Virtual Economy: Users have a virtual balance that can be deposited into and used to spin the slots.

    Transaction History: A dedicated economy page (/economies) to view past spins, wins, and deposits.

    Slot Machine: A JavaScript-powered frontend slot machine that communicates with the Ruby backend to process bets and wins based on server-side odds.

    Jackpots: Users can buy into active jackpots. A portion of the buy-in price is added to the total jackpot value.

    Account Management: Users can delete their own accounts.

🛡️ Admin Features

    Live Odds Modification: Admins can change the win probability / odds of the slot machine in real-time, affecting all current players.

    Jackpot Management: Admins can create new jackpots or delete/payout existing ones. Payouts are automatically distributed evenly among all participating players.

🛠️ Tech Stack

    Frontend: HTML5, CSS3, JavaScript (Vanilla)

    Backend: Ruby, Sinatra (Web Framework)

    Database: SQLite3

    Authentication/Security: bcrypt, securerandom, rack-flash

🚀 Getting Started
Prerequisites

    Ruby (v2.7+ recommended)

    Bundler (gem install bundler)

    SQLite3 installed on your machine

The database seeder provisions two default accounts for testing purposes:
Role	Username	Password	Starting Balance
Admin	admin	admin	1000
User	user	user	1000
📂 Project Structure
code Text

├── app.rb                # Main application file & routing logic
├── seeder.rb             # Database initialization and seeding script
├── db/                   # Directory containing the SQLite3 database
│   └── casino.sqlite
├── models/               # Database interaction models
│   ├── economy.rb
│   ├── jackpot_users.rb
│   ├── jackpots.rb
│   ├── stats.rb
│   └── users.rb
├── views/                # ERB view templates (HTML)
│   ├── economies/
│   ├── hell/             # Slot machine views
│   ├── sessions/         # Login views
│   └── users/            # Registration views
└── public/               # Static frontend assets (CSS, JS, Images)

🔗 API / Routing Overview

Authentication & Users

    GET /login / POST /login - Handle user sessions

    POST /logout - Destroy user session

    GET /users/new / POST /users - Account creation

    POST /users/update - Deposit funds

    POST /users/:id/delete - Delete account

Gameplay & Economy

    GET /slot - Main slot machine interface

    POST /users/update_spin - Processes a spin (deducts bet, adds win)

    GET /economies - User transaction history

    GET /stats - Fetches current slot odds (JSON)

    POST /stats/update - Admin updates to slot odds

Jackpots

    GET /jackpots - Fetches active jackpots (JSON)

    POST /jackpots/create - Admin creates a jackpot

    POST /jackpot_users - User buys into a jackpot

    POST /jackpots/:id/delete - Admin deletes/pays out a jackpot


🔍 Security & Code Quality Retrospective

Looking back at this project with my current background in cybersecurity and software development, several architectural patterns and vulnerabilities serve as valuable learning points:

🛡️ Security

    * Client-Side Trust (CWE-602): In `/users/update_spin`, the backend trusts the frontend payload for the `win` calculation. An attacker can intercept the HTTP request (e.g., using Burp Suite) and inject arbitrary win amounts. In a production environment, RNG evaluation, payout math, and balance updates must strictly execute server-side within isolated, database transactions.

    * Input Validation & Edge Cases: The deposit endpoint (`/users/update`) lacks boundary enforcement, permitting negative amounts or extreme numbers that trigger floating-point overflows (IEEE 754 float overflow to `$Infinity`). Proper input validation should reject negative values and enforce strict numeric bounds.

    * Cookie-Based Rate Limiting Bypass: The brute-force protection on `/login` stores attempt counters inside the client session cookie. An attacker can simply clear their cookies between requests to reset the lock-out counter. Rate-limiting should be stored server-side (e.g., in Redis or a DB table mapped to IP/identity).

    * Denial of Service (ZeroDivisionError): In `/jackpots` (`weight = 1000000 / jackpot["value"]`) and `/jackpots/:id/delete`, missing zero-checks can trigger unhandled division-by-zero exceptions, returning HTTP 500 errors if values or participant counts are zero.
    

🧹 Code Quality & Maintainability

    * Domain-Driven Naming: Ambiguous variable names in the same API scope (e.g., `jackpot_list` vs. `jackpot_array`) should be renamed semantically to reflect their domain context.

    * Database Optimization: Nested in-memory loops used to correlate jackpots and users  should be delegated to the database engine using SQL `JOIN` operations.

    * Precise Error Handling: Conflated error logic (such as returning `"User not found"` when a user exists but has a balance `< 20`) should be separated into explicit 404 (Not Found) vs. 400 (Insufficient Balance) responses.

    * Missing Comments & Documentation: The codebase is largely uncommented. It lacks function headers/docstrings and inline explanations for complex game and jackpot logic, making it harder to read and maintain.