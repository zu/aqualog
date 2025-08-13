# Ägerisee Wetter - Weather Dashboard

A beautiful weather dashboard for Lake Ägeri with user authentication and data management features.

## Features

- **Real-time Weather Data**: Water temperature, air temperature, and weather conditions
- **User Authentication**: Login with Google, GitHub, or email/password
- **User Management**: Save personal data, locations, and temperature alerts
- **Admin Panel**: Manage users and view system statistics
- **Temperature History**: Visual charts showing water temperature trends
- **Responsive Design**: Works on all devices with a modern, glassmorphism UI

## Setup Instructions

### 1. Firebase Configuration

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project
3. Enable Authentication:
   - Go to Authentication > Sign-in method
   - Enable Google, GitHub, and Email/Password providers
   - For GitHub, you'll need to create a GitHub OAuth app
4. Enable Firestore Database:
   - Go to Firestore Database
   - Create database in test mode
5. Get your config:
   - Go to Project Settings > General
   - Scroll down to "Your apps" section
   - Click the web icon (</>)
   - Register your app and copy the config

### 2. Update Configuration

Replace the Firebase config in `index.html` with your actual values:

```javascript
const firebaseConfig = {
  apiKey: "your-actual-api-key",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-actual-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "your-actual-sender-id",
  appId: "your-actual-app-id"
};
```

### 3. GitHub OAuth Setup (Optional)

If you want GitHub login:
1. Go to GitHub Settings > Developer settings > OAuth Apps
2. Create new OAuth App
3. Set Authorization callback URL to: `https://your-project.firebaseapp.com/__/auth/handler`
4. Copy Client ID and Client Secret to Firebase GitHub provider

### 4. Deploy to GitHub Pages

1. Push your code to GitHub
2. Go to repository Settings > Pages
3. Select source branch (usually `main`)
4. Your site will be available at: `https://username.github.io/repository-name`

## Security Rules

Update your Firestore security rules:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Admins can read all user data
    match /users/{userId} {
      allow read: if request.auth != null && 
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
    
    // Users can read/write their temperature history
    match /users/{userId}/temperatureHistory/{document} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## User Roles

- **User**: Default role, can save personal data and view their history
- **Admin**: Can manage all users, view system statistics, and change user roles

## Data Storage

The app stores:
- User profiles and roles
- Saved locations and temperature alerts
- Personal temperature history
- System usage statistics

## API Endpoints Used

- **Water Temperature**: `https://api.datalakes-eawag.ch/data/1074/y`
- **Air Temperature**: `https://api.datalakes-eawag.ch/data/1073/y`
- **Historical Data**: `https://api.datalakes-eawag.ch/data/1077/h`
- **Weather**: OpenWeatherMap API

## Browser Support

- Chrome/Edge: Full support
- Firefox: Full support
- Safari: Full support (with webkit prefixes)
- Mobile browsers: Full responsive support

## Troubleshooting

### Common Issues

1. **Firebase not initialized**: Check your config values
2. **Authentication not working**: Verify providers are enabled in Firebase
3. **Database errors**: Check Firestore security rules
4. **CORS issues**: Ensure your domain is whitelisted in Firebase

### Debug Mode

Open browser console (F12) to see detailed error messages and API responses.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is open source and available under the MIT License. 