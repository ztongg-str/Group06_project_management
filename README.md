# Getting Started

## 1. Create the `.env` file

Inside the `back` folder, create a file named `.env` and fill in your own values:

```env
DB_HOST=your-database-host
DB_PORT=3306
DB_USER=your-db-user
DB_PASSWORD=your-db-password
DB_NAME=unipulse
PORT=4000
JWT_SECRET=generate-a-long-random-secret
JWT_EXPIRES_IN=1d
BCRYPT_ROUNDS=10
NODE_ENV=development
```

> **Never commit `.env` or real credentials to the repository.**

---

## 2. Start the Backend

Open a terminal and run:

```bash
cd back
npm install express
npm run dev
```

---

## 3. Start the Frontend

Open a **second terminal** and run:

```bash
cd front
npm install
npm run dev
```

---

## 4. Open the Application

Once both servers are running, open your browser and visit:

```text
http://localhost:5173
```
