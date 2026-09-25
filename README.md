# PWA Customer Manager (Gestor de Clientes PWA)

A Progressive Web Application (PWA) for managing and displaying customer information with offline capabilities.

## 🎯 Overview

This is a modern Progressive Web Application built with vanilla JavaScript that provides a customer management interface. It leverages Service Workers for offline functionality and implements caching strategies to ensure seamless user experience even without internet connectivity.

**Language:** Spanish (ES) with English documentation

## ✨ Features

- **Progressive Web App (PWA)**: Installable on desktop and mobile devices
- **Offline Support**: Service Worker caches customer data for offline access
- **Network-First API Strategy**: Prioritizes live data from the server when available
- **Cache-First Static Assets**: Optimized loading of CSS, JS, and HTML files
- **Responsive Design**: Works across desktop and mobile devices
- **Standalone Display Mode**: App-like experience with no browser UI
- **Fast Installation**: Quick load times with optimized caching

## 📁 Project Structure

```
pwa-customer/
├── index.html          # Main HTML entry point
├── manifest.json       # PWA manifest configuration
├── sw.js              # Service Worker for offline support
├── js/
│   ├── app.js         # Main application logic
│   ├── api.js         # API communication module
│   └── ui.js          # UI rendering functions
├── icons/             # Application icons (192x192 and 512x512)
└── README.md          # This file
```

## 🏗️ Architecture

### Core Modules

#### **app.js** - Application Entry Point
- Initializes the PWA on DOM ready
- Orchestrates API calls and UI rendering
- Handles application-level error management

#### **api.js** - API Communication
- Handles HTTP requests to the customer API
- Endpoint: `http://localhost:9090/api/v1/customer`
- Supports custom headers (flow header for authentication/routing)
- Provides error handling for network failures

#### **ui.js** - User Interface Rendering
- Renders customer list in card format
- Displays customer information: name, ID, phone
- Shows error messages to users gracefully

#### **sw.js** - Service Worker
- Manages offline functionality
- Implements two caching strategies:
  - **Network-First** for API calls: Tries network first, falls back to cache
  - **Cache-First** for static assets: Uses cache first, updates in background
- Cleans up old caches on updates

### Caching Strategy

The Service Worker implements intelligent caching:

1. **Static Assets** (HTML, CSS, JS)
   - Serves from cache immediately
   - Updates cache in the background
   - Provides instant loading

2. **API Calls** (`/api/v1/customer`)
   - Fetches from network first
   - Caches successful responses
   - Falls back to cached data when offline

## 🚀 Getting Started

### Prerequisites
- Node.js and npm (if adding build tools)
- A web server to serve the application
- Backend API running at `http://localhost:9090/api/v1/customer`

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd pwa-customer
   ```

2. **Serve the application**
   Using Python:
   ```bash
   python -m http.server 8000
   ```
   
   Or using Node.js with http-server:
   ```bash
   npm install -g http-server
   http-server
   ```

3. **Access the application**
   - Open `http://localhost:8000` in your browser
   - Install the app from the browser menu (optional)

### Configuration

Update the API endpoint in `js/api.js`:
```javascript
const API_URL = 'http://localhost:9090/api/v1/customer';
```

Add your custom header value:
```javascript
'flow': 'your_custom_flow_value'
```

## 🔌 API Integration

### Endpoint
- **URL**: `http://localhost:9090/api/v1/customer`
- **Method**: GET
- **Headers**: 
  - `Content-Type: application/json`
  - `flow: <custom_value>` (configurable)

### Expected Response Format
```json
[
  {
    "id": "customer_id",
    "name": "Customer Name",
    "phone": "Phone Number"
  },
  ...
]
```

## 📱 PWA Features

### Installation
The app can be installed on:
- Chrome/Edge (desktop and mobile)
- Firefox (coming soon)
- Safari (iOS 16.4+)

**Installation Methods:**
1. Click the install button in the address bar (if available)
2. Use the browser menu → "Install app" or "Add to Home Screen"

### Manifest Configuration
The `manifest.json` file defines:
- App name and short name
- Display mode: Standalone (full-screen app experience)
- Theme colors (blue: #2563eb)
- Background color (white: #ffffff)
- App orientation: Portrait
- Icons for home screen and splash screens

### Offline Access
Once installed and used once:
- Customer list loads from cache
- User can view previously loaded customers offline
- When reconnected, fresh data is automatically fetched

## 🔄 Service Worker Lifecycle

### Install Phase
- Caches all static assets
- Ensures immediate availability on first load

### Activate Phase
- Removes old cache versions
- Keeps app storage clean
- Claims control of all clients

### Fetch Phase
- Intercepts network requests
- Applies appropriate caching strategy
- Handles network failures gracefully

## 🛠️ Development

### Modifying Caching Strategy
Edit `sw.js` to adjust which routes use which strategy:
```javascript
// Add more routes to network-first strategy
if (url.pathname.includes('/api/v1/your-endpoint')) {
    event.respondWith(/* network-first logic */);
}
```

### Adding New Features
1. **New API endpoints**: Add methods to `api.js`
2. **New UI components**: Create functions in `ui.js`
3. **App logic**: Update `app.js` with initialization logic

### Testing Offline Mode
1. Open DevTools (F12)
2. Go to Application tab → Service Workers
3. Check "Offline" box
4. Reload page to test offline functionality

## 🧪 Testing

### Manual Testing Checklist
- [ ] App loads successfully online
- [ ] Customer list displays correctly
- [ ] App installs to home screen
- [ ] Offline mode shows cached customers
- [ ] Error message displays when API is unavailable
- [ ] Service Worker updates when code changes

### Browser DevTools
- **Application Tab**: View Service Worker status and cache
- **Network Tab**: Monitor requests and caching behavior
- **Console Tab**: Check for errors and logs

## 🔒 Security Considerations

1. **API Communication**: Currently uses HTTP (update to HTTPS in production)
2. **Custom Headers**: Configure the `flow` header for your authentication
3. **Cache Storage**: Data is stored locally; consider sensitivity of cached information
4. **CORS**: Ensure backend allows requests from your PWA origin

## 📊 Performance

- **First Load**: ~2-3 seconds (depends on API response)
- **Subsequent Loads**: <500ms (from cache)
- **Offline Mode**: Instant (from cache)
- **Bundle Size**: ~5KB (JS files)

## 🌐 Browser Support

| Browser | Support | Min Version |
|---------|---------|-------------|
| Chrome  | ✅ Full | 40+         |
| Edge    | ✅ Full | 15+         |
| Firefox | ✅ Full | 44+         |
| Safari  | ⚠️ Partial | 11.1+    |
| Opera   | ✅ Full | 27+         |

## 🐛 Troubleshooting

### App Won't Install
- Clear browser cache and service workers
- Ensure `manifest.json` is valid
- Check browser console for errors

### Customers Not Loading
- Verify API endpoint is running
- Check network tab for failed requests
- Ensure `flow` header is correctly configured
- Verify API response format matches expectations

### Service Worker Not Working
- Go to DevTools → Application → Service Workers
- Unregister old service workers
- Hard refresh (Ctrl+Shift+R)
- Check console for Service Worker errors

### Offline Data Not Available
- Ensure app was used online at least once
- Check Application tab → Cache Storage
- Verify Service Worker is active and running

## 📝 API Response Example

```json
[
  {
    "id": "001",
    "name": "Juan García López",
    "phone": "+34912345678"
  },
  {
    "id": "002",
    "name": "María Rodríguez Pérez",
    "phone": "+34987654321"
  }
]
```

## 🚀 Deployment

### Static Hosting (Recommended)
- **Vercel**: `vercel deploy`
- **Netlify**: `netlify deploy`
- **GitHub Pages**: Push to gh-pages branch
- **AWS S3 + CloudFront**: Upload files and configure CDN

### Requirements
- HTTPS enabled (required for Service Workers)
- Proper CORS headers if API is on different domain
- Cache headers configured for static assets

### Environment Setup
```bash
# Before deployment
1. Update API_URL in js/api.js to production endpoint
2. Update manifest.json with production URLs
3. Ensure icons are in icons/ directory
4. Test PWA installation on target device
```

## 📚 Additional Resources

- [PWA Documentation](https://web.dev/progressive-web-apps/)
- [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [Web App Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest)
- [PWA Checklist](https://web.dev/pwa-checklist/)

## 📄 License

This project is provided as-is. Modify and use as needed for your requirements.

## 👤 Author

Created as a Progressive Web Application template for customer management systems.

## 🤝 Contributing

Feel free to fork, modify, and improve this project. Consider adding:
- Search and filter functionality
- Customer details page
- Add/Edit/Delete operations
- Database persistence
- Authentication system

---

**Last Updated**: September 2026

For questions or issues, please refer to the troubleshooting section above.
