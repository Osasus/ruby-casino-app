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

1. Clone the repository
code Bash

git clone <your-repository-url>
cd <your-repository-folder>

2. Install Dependencies

Ensure you have a Gemfile with the required gems (like sinatra, sqlite3, bcrypt, rack-flash3, sinatra-activerecord), then run:
code Bash

bundle install

3. Setup the Database

Create a db folder in the root directory (if it doesn't exist) so the database can be generated properly:
code Bash

mkdir db

Run the provided seeder file to drop/create tables and populate default data:
code Bash

ruby seeder.rb

4. Run the Application

Start the Sinatra server:
code Bash

ruby app.rb

The app should now be running locally. Open your browser and navigate to http://localhost:4567.
🔐 Default Credentials

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