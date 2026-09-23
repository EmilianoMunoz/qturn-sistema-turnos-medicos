# QTurn mobile client

Expo/React Native client for QTurn's medical appointment platform. It provides role-based flows for administrators, doctors, and patients and communicates with the Spring Boot API using JWT bearer authentication.

## Setup

```bash
cp .env.example .env
npm ci
npm start
```

Configure `EXPO_PUBLIC_API_URL` in `.env`. See the [root project documentation](../../README.md) for Docker setup, emulator-specific URLs, architecture, and backend configuration.

## Commands

```bash
npm start          # Start Expo
npm run android    # Open Android target
npm run ios        # Open iOS target
npm run web        # Open web target
npm test           # Run Jest once
npm run test:watch # Run Jest in watch mode
npm run lint       # Run Expo lint
```

Authentication tokens and user identifiers are stored with Expo SecureStore. Do not commit local `.env` files or real credentials.
