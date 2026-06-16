# 🗺️ ICMR Smart Navigation — AR-Powered Indoor Wayfinding

An augmented reality-based indoor navigation system for the ICMR (Indian Council of Medical Research) headquarters. This project uses **MindAR** image tracking and **A-Frame** WebXR to provide real-time visual guidance through the building corridors.

---

## 🎯 Features

- **Marker-based AR Navigation**: Uses printed physical markers (A, B, C) as waypoints throughout the building
- **Multi-step Guidance**: Progressive navigation with 3 distinct steps:
  1. **Entrance Detection** — Scan Marker A to begin navigation
  2. **Corridor Navigation** — Follow Marker B for in-corridor guidance  
  3. **Final Destination** — Use Marker C for final turns and room identification
- **Real-time HUD Overlay**: 
  - Step-by-step instructions
  - Distance indicators
  - Progress tracking with animated dots
  - Live marker detection feedback
- **3D Arrow Guidance**: Color-coded animated arrows that point directions:
  - 🟢 **Green arrows** = Walk straight
  - 🟠 **Orange arrows** = Turn right
- **Responsive Design**: Optimized for mobile devices with viewport control
- **Smooth Transitions**: Loading screens, detection flashes, and arrival confirmation
- **Voice-friendly Interface**: Clear text labels for each navigation step

---

## 🏗️ How It Works

### Architecture
```
Home Screen → Loading Screen → AR Scene (with HUD) → Arrival Screen
```

### Technology Stack
- **A-Frame 1.5.0** — WebXR framework for 3D scenes
- **MindAR 1.2.5** — Image-based target tracking
- **Vanilla JavaScript** — No build tool required
- **CSS3** — Modern responsive styling with CSS variables
- **HTML5** — Semantic markup

### Navigation Flow

1. **User selects destination** on home screen (Dr. Nilesh Chandra's office, Room 120-A)
2. **Loading screen appears** while AR engine initializes
3. **User points camera at Marker A** at the entrance gate
4. **AR scene activates** with 3D directional arrow and HUD overlays
5. **System detects Marker B** and updates instructions
6. **System detects Marker C** and provides final turn instruction
7. **Arrival confirmation screen** appears with destination details

---

## 📋 File Structure

```
icmr-nav2/
├── index.html          # Main application (all-in-one)
├── targets.mind        # MindAR target database (compiled from marker images)
├── marker-A.png        # Entrance marker image
├── marker-B.png        # Corridor marker image
├── marker-C.png        # End-of-corridor marker image
└── README.md           # Documentation
```

### Key Components in index.html

- **Styles** (lines 12–117): 
  - Responsive grid layouts with CSS variables
  - Screen management (hidden/active states)
  - HUD overlay positioning
  - Animation keyframes (spinner, pulse rings)

- **Home Screen** (lines 124–159):
  - Logo ring with SVG icon
  - Destination button with room details
  - Usage tip box

- **AR Scene** (lines 173–238):
  - A-Frame scene with MindAR image targets
  - 3D arrow primitives (cylinders, cones, torus)
  - Text labels and destination cards
  - HUD overlay for instructions and progress

- **JavaScript Logic** (lines 284–373):
  - `STEPS` array: Contains instructions for each navigation step
  - `selectDest()`: Starts AR initialization
  - `initAR()`: Loads MindAR and sets up event listeners
  - `onMarker()`: Detects marker presence and advances navigation
  - `updateHUD()`: Updates visible instructions and progress dots
  - `showFlash()`: Displays detection confirmation

---

## 🚀 Getting Started

### Prerequisites
- Modern mobile browser with camera access (Chrome, Firefox, Safari on iOS 14.5+)
- Printed marker images at designated locations
- HTTPS connection (required for camera access)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Chirag-Sharmaaa/icmr-nav2.git
   cd icmr-nav2
   ```

2. **Generate MindAR targets**
   - Use [MindAR Target Compiler](https://visionnav.com/studio) to compile `marker-A.png`, `marker-B.png`, and `marker-C.png` into `targets.mind`
   - Place the compiled `targets.mind` file in the repository root

3. **Print physical markers**
   - Print the marker PNGs at physical locations:
     - **Marker A**: Main entrance gate (~A4 size)
     - **Marker B**: At corridor entry (~A4 size)
     - **Marker C**: End of corridor where turn occurs (~A4 size)

4. **Deploy to a web server**
   ```bash
   # Using Python
   python -m http.server 8000
   
   # Using Node.js
   npx serve
   ```

5. **Access the application**
   - Open `https://localhost:8000` on a mobile device
   - Grant camera permission when prompted

---

## 🎨 Customization

### Change Destination
Edit lines 288–291 in `index.html`:
```javascript
const STEPS = [
  { label:'Step 1 of 3', instr:'Your instruction here', dist:'~Xm', prompt:'Your prompt', promptSub:'Sub-instruction' },
  // ...
];
```

### Modify Colors
Edit the CSS variables at line 14–18:
```css
:root{
  --blue:#003580;      /* Primary */
  --blue2:#0057B7;     /* Secondary */
  --green:#00C49A;     /* Success/forward */
  --yellow:#FFAA00;    /* Alert/turn */
  --white:#fff;
  --surface:#F0F4FF;
  --text:#1A1A2E;
  --muted:#6B7A99;
}
```

### Adjust 3D Elements
Arrow position, size, and animation can be modified in lines 187–236 (A-Frame entity definitions):
```html
<a-cylinder color="#00C49A" position="0 0.3 -0.5" radius="0.04" height="0.7"></a-cylinder>
```

---

## 📍 Marker Setup Guide

### Marker Requirements
- **Format**: PNG with high contrast
- **Size**: Print at least A4 (210mm × 297mm)
- **Quality**: Markers must be sharp and well-lit
- **Placement**: Mount securely on walls at eye level

### Step-by-Step Placement

1. **Marker A** — Entrance Gate
   - Position: Main entrance at ~2m height
   - Purpose: Initialize navigation
   - User position: Standing at entrance

2. **Marker B** — Corridor Entry
   - Position: After reception, corridor entry at ~2m height
   - Purpose: Confirm forward progress
   - User position: ~10m from Marker A

3. **Marker C** — Corridor End
   - Position: Where turn is required, at ~2m height
   - Purpose: Trigger final turn instruction
   - User position: ~21m from Marker B

---

## 🧪 Testing

### Local Testing
```bash
# Start local HTTPS server
npx http-server -p 8080 -S

# Open on mobile device: https://<your-ip>:8080
```

### Marker Detection Debugging
1. Open browser console (F12)
2. Check for MindAR initialization logs
3. Verify `targets.mind` file is loading (Network tab)
4. Test marker recognition with printed images

---

## 🔧 Troubleshooting

| Issue | Solution |
|-------|----------|
| Camera not loading | Check HTTPS, grant camera permission, try different browser |
| Markers not detected | Ensure printed markers are sharp, well-lit, and in frame |
| AR scene freezes | Refresh page, check device performance, close other apps |
| Blurry navigation | Ensure camera focus, move markers to optimal distance (~30cm) |
| Navigation jumps steps | Verify `onMarker()` logic and step progression in STEPS array |

---

## ��� Dependencies

- **A-Frame** — 3D scene framework (1.5.0)
- **MindAR** — Image tracking engine (1.2.5)
- **CDN Hosted** — No build step required

All dependencies are loaded from CDN. No npm installation needed.

---

## 👤 Customization for Other Buildings

To adapt this for a different building:

1. **Change building name** (line 133): `<h1>Your Building Smart Navigation</h1>`
2. **Update destination** (lines 141–153): Edit destination button details
3. **Modify navigation steps** (lines 288–291): Update STEPS array with your route
4. **Generate new markers** (lines 187–236): Change marker positions and instructions
5. **Create new printed markers** (marker-A.png, marker-B.png, marker-C.png)
6. **Compile targets.mind** from new markers

---

## 📄 License

This project is open source. Feel free to use, modify, and distribute.

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/YourFeature`)
3. Commit changes (`git commit -m 'Add YourFeature'`)
4. Push to branch (`git push origin feature/YourFeature`)
5. Open a Pull Request

---

## 📞 Support

For issues, questions, or feature requests, please open an [issue on GitHub](https://github.com/Chirag-Sharmaaa/icmr-nav2/issues).

---

## 🎓 Learn More

- [A-Frame Documentation](https://aframe.io/)
- [MindAR Documentation](https://hiukim.github.io/mind-ar-js-doc/)
- [WebXR Guide](https://www.khronos.org/webxr/)

---

**Built with ❤️ for seamless indoor navigation**
