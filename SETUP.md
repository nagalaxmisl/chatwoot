# Chatwoot Local Setup Guide

This guide will help you set up Chatwoot on your local machine from a fresh fork.

Follow the steps below to install prerequisites, configure the environment, and run the application locally.

---

## Prerequisites

Make sure the following dependencies are installed on your system:

- Git
- Node.js (v18+)
- Yarn (v1.22+)
- PostgreSQL (v14+)
- Redis
- Ruby (v3.2+ via rbenv or rvm)
- Bundler (`gem install bundler`)
- Foreman (`gem install foreman`)
- pnpm (`npm install -g pnpm`)

---

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/nagalaxmisl/chatwoot.git
cd chatwoot
```

### 2. Install Backend dependencies
```bash
bundle install
```

### 3. Install Frontend Dependencies
```bash
pnpm install
```

---

### Environment Setup

### 4. Copy and Configure `.env`

```bash
cp .env.example .env

```

Edit the `.env` file and update the following environment variables:

#### SECRET_KEY_BASE

Generate a secure secret using the command:

```bash
bundle exec rake secret
```

#### REDIS_URL

```env
REDIS_URL=redis://localhost:6379
```

#### PostgreSQL Configuration

```env
POSTGRES_HOST=localhost
POSTGRES_USERNAME=your_pg_username
POSTGRES_PASSWORD=your_pg_password
```

---

## Database Setup

### 5. Set up the database

Make sure PostgreSQL is running, then run:

```bash
rails db:setup
```

---

## Start the App

### 6. Run the application locally

Use Foreman to start the backend and frontend services:

```bash
foreman start -f Procfile.dev
Once the app is running, open:

- http://localhost:3000 – Chatwoot UI
- http://localhost:3036/vite-dev/ – Vite frontend dev server
```

---

##  Test Core Functionality

### 7. Create an Admin User

Run the following in the Rails console:

```bash
bundle exec rails c
```

```ruby
user = User.create!(
  email: 'admin@example.com',
  password: 'Admin@123',
  password_confirmation: 'Admin@123',
  name: 'Admin User',
  confirmed_at: Time.zone.now
)

account = Account.first
AccountUser.create!(account_id: account.id, user_id: user.id, role: :administrator)
```

---

## Common Issues

- **Redis connection error**
Make sure Redis is running:
```bash
redis-server
```

- **PostgreSQL authentication error**
Double-check that your `.env` has the correct username and password.

- **Port already in use**
If the server doesn’t start, try removing the old PID file:
```bash
rm -f tmp/pids/server.pid
```

---

## Notes

- This `SETUP.md` is created for internal documentation and should **not** be edited upstream.
- Update this file if you change environment versions, ports, or dependencies.
- Always run `bundle install` and `pnpm install` after pulling new changes.
- For advanced debugging or contribution guidelines, refer to the official [Chatwoot Docs](https://www.chatwoot.com/docs).



