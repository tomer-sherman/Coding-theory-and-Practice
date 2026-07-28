## 1. Create and enter the folder

```bash
mkdir northwind-rest-api
cd northwind-rest-api
```

## 2. Initialize `package.json` — always first

```bash
npm init -y
```

Never hand-create this file. Every npm command parses it before doing anything, so an empty or malformed one blocks even `npm init` itself.

## 3. Install dependencies — two commands

```bash
npm install colors dotenv express mysql2 zod
npm install -D @types/express @types/node nodemon tsx
```

Runtime deps first, then dev deps. Separate commands because `-D` applies to the whole line.

## 4. Create `tsconfig.json`

```json
{
    "compilerOptions": {
        "target": "esnext",
        "module": "nodenext",
        "moduleResolution": "nodenext",
        "esModuleInterop": true,
        "strict": true,
        "types": ["node"]
    }
}
```

## 5. Add the script to `package.json`

```json
"scripts": {
  "start": "nodemon --exec tsx src/app.ts --quiet"
}
```

Then `npm start` is the only command you need. (`--quiet` suppresses nodemon's own banner output so you only see your app's logs.)

## 6. Create `.gitignore`

```
node_modules/
.env
```

## 7. Create `.env`

```
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=yourpassword
DB_NAME=northwind
```

## 8. Build the folder structure

```bash
mkdir src
mkdir src/controllers src/middleware src/models src/services src/util
```

```
northwind-rest-api/
├── src/
│   ├── controllers/   ← request handlers
│   ├── middleware/    ← error handling, logging, validation
│   ├── models/        ← data shapes + zod schemas
│   ├── services/      ← business logic + DB queries
│   ├── util/          ← helpers, db connection, config
│   └── app.ts         ← entry point
├── .env
├── .gitignore
├── package.json
└── tsconfig.json
```

## 9. Run it

```bash
npm start
```