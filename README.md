# SimpleAppGallery - PWA Media Gallery

A lightweight, high-performance Progressive Web App (PWA) designed to curate and view media (images and videos) directly from URLs. Built with pure Vanilla JS, CSS3, and HTML5, it offers a seamless, app-like experience on both desktop and mobile devices.

## ✨ Features
- **URL-Based Gallery**: Easily manage your media by pasting lists of URLs.
- **Support for Images & Videos**: Automatically detects and renders images or videos with optimized settings.
- **PWA Ready**: Installable on Android, iOS, and Desktop. Works offline with Service Worker caching.
- **Fluid Navigation**: Supports keyboard arrows, screen clicks, and touch gestures (swipe).
- **Media Preloading**: Background loading of the next items for zero-latency browsing.
- **Adaptive UI**: Dark-themed, responsive design that adapts to any screen size (Fit/Fill modes).
- **Media Evaluation**: Built-in system to mark media as OK or NOK (thumbs up/down).
- **Data Export**: Export your evaluations as a JSON file.

## 🚀 Getting Started
1. **Clone the repository**:
   ```bash
   git clone https://github.com/Tyrin451/SimpleAppGallery.git
   cd SimpleAppGallery
   ```
2. **Serve the application**:
   Since it is a PWA, it must be served over HTTP(S). You can use any static server. For example, using Python:
   ```bash
   python -m http.server
   ```
3. **Access the app**:
   Open your browser at `http://localhost:8000`.

## 🛠 Usage
### Configuration
1. Tap the **Configuration** button in the bottom navigation bar.
2. Paste your media URLs into the text area (one URL per line).
3. Tap **CHARGER** (Load).

### Gallery
- Click or tap the **right side** of the screen (or use **Right Arrow**) to go to the next media.
- Click or tap the **left side** of the screen (or use **Left Arrow**) to go to the previous media.
- Swipe **left or right** on mobile devices.
- Use the **Thumbs Up/Down** buttons (or **Up/Down Arrows**) to evaluate media.
- Tap the **FIT/FILL** button to toggle between image scaling modes.
- Tap **TÉLÉCHARGER JSON** (Download) in the configuration view to export your evaluations.

## 📦 Project Structure
- `index.html`: The main entry point containing HTML structure, styles, and application logic.
- `sw.js`: Service Worker for offline support and asset caching.
- `manifest.json`: PWA configuration for installation and look-and-feel.
- `Specifications.md`: Detailed technical specifications (in French).

## 🔧 Technical Details
- **Stack**: Vanilla JS (ES6+), CSS3 (Flexbox/Grid), HTML5.
- **Persistence**: `localStorage` is used to store URLs and evaluation states.
- **Performance**: Implements media preloading and efficient video lifecycle management (auto-pause/unload).