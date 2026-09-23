# Aplicación móvil de QTurn

Cliente desarrollado con Expo y React Native para la plataforma de turnos médicos QTurn. Incluye flujos diferenciados para administradores, médicos y pacientes, y se comunica con la API Spring Boot mediante autenticación JWT.

## Configuración

```bash
cp .env.example .env
npm ci
npm start
```

Configurar `EXPO_PUBLIC_API_URL` en el archivo `.env`. La [documentación principal](../../README.md) contiene las instrucciones para Docker, las direcciones correspondientes a cada emulador y la configuración del backend.

## Comandos disponibles

```bash
npm start          # Iniciar Expo
npm run android    # Abrir la aplicación en Android
npm run ios        # Abrir la aplicación en iOS
npm run web        # Abrir la aplicación web
npm test           # Ejecutar Jest una vez
npm run test:watch # Ejecutar Jest en modo observación
npm run lint       # Ejecutar el análisis estático de Expo
```

Los tokens de autenticación y los identificadores de usuario se almacenan con Expo SecureStore. Los archivos `.env` locales y las credenciales reales no deben incorporarse al repositorio.
