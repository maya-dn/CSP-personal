# Escape the Basement - Laundry Minigame

## Game Description
A drag-and-drop puzzle minigame where players must:
1. Repair a broken washing machine by placing parts
2. Load dirty laundry into the machine
3. Run a wash cycle
4. Discover a secret code hidden in the machine

## Project Structure
```
my-game/
├── index.html              # Main game page
├── laundry_minigame.js    # Minigame JavaScript
├── README.md              # This file
└── images/
    ├── basement.png
    ├── broken-washing-machine-jpeg.jpeg
    ├── motor.png
    ├── belt.png
    ├── pump.jpg
    ├── hose.png
    ├── shirt.png
    ├── pants.png
    ├── socks.png
    ├── towel.png
    └── jacket.png
```

## Setup Instructions

### 1. Create the HTML file (index.html)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Escape the Basement Game</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background: #1a1a1a;
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        #start-btn {
            padding: 20px 40px;
            font-size: 20px;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
        }
        #start-btn:hover {
            background: #45a049;
        }
    </style>
</head>
<body>
    <button id="start-btn">Start Laundry Minigame</button>

    <script src="laundry_minigame.js"></script>
    
    <script>
        document.getElementById('start-btn').addEventListener('click', function() {
            showLaundryMinigame(() => {
                console.log('Minigame completed!');
                alert('You found the code: 4-7-2-9!');
            });
        });
    </script>
</body>
</html>
```

### 2. Add the JavaScript file (laundry_minigame.js)
See the full code in the `laundry_minigame.js` file

### 3. Add Your Images
Place all sprite images in the `images/` folder with these exact names:
- `basement.png` - Background
- `broken-washing-machine-jpeg.jpeg` - Washing machine
- `motor.png`, `belt.png`, `pump.jpg`, `hose.png` - Machine parts
- `shirt.png`, `pants.png`, `socks.png`, `towel.png`, `jacket.png` - Laundry items

### 4. Update Image Paths in JavaScript
In `laundry_minigame.js`, update all image paths to include `images/`:
```javascript
// Line ~47
container.style.backgroundImage = "url('images/basement.png')";

// Line ~165
machineContainer.style.backgroundImage = "url('images/broken-washing-machine-jpeg.jpeg')";

// Lines ~145-148 (in partsList array)
const partsList = [
    { name: 'Motor', type: 'motor', sprite: 'images/motor.png' },
    { name: 'Belt', type: 'belt', sprite: 'images/belt.png' },
    { name: 'Pump', type: 'pump', sprite: 'images/pump.jpg' },
    { name: 'Hose', type: 'hose', sprite: 'images/hose.png' }
];

// Lines ~345-349 (in laundryList array)
const laundryList = [
    { name: 'Shirt', type: 'shirt', sprite: 'images/shirt.png' },
    { name: 'Pants', type: 'pants', sprite: 'images/pants.png' },
    { name: 'Socks', type: 'socks', sprite: 'images/socks.png' },
    { name: 'Towel', type: 'towel', sprite: 'images/towel.png' },
    { name: 'Jacket', type: 'jacket', sprite: 'images/jacket.png' }
];
```

## Running the Game

### Method 1: Python Local Server (Recommended)
```bash
cd /path/to/my-game
python -m http.server 8000
```
Then open: `http://localhost:8000/index.html`

### Method 2: Direct File Access
Simply drag and drop `index.html` into your browser, or navigate to:
```
file:///C:/path/to/your/my-game/index.html
```

### Method 3: XAMPP/WAMP
- Place folder in `htdocs` or `www`
- Access at: `http://localhost/my-game/index.html`

## How to Play

1. **Phase 1: Repair the Machine**
   - Drag each part (Motor, Belt, Pump, Hose) to its correct position on the washing machine
   - Parts must match the labeled drop zones

2. **Phase 2: Load the Laundry**
   - Once repaired, drag all 5 laundry items into the machine door
   - All items must be loaded before proceeding

3. **Phase 3: Run the Cycle**
   - Click "Start Laundry Cycle"
   - Watch the washing animation

4. **Phase 4: Discover the Code**
   - Find the secret paper with the basement exit code: **4-7-2-9**
   - Click "Take the Paper" to complete the minigame

## Customization

### Change the Secret Code
In `laundry_minigame.js`, find this line (around line 475):
```javascript
<span style="font-size: 20px; font-weight: bold; color: #ff0000;">4 - 7 - 2 - 9</span>
```
Change the numbers to your desired code.

### Add Custom Paper Image
Uncomment line ~461 and add your image:
```javascript
paperImage.style.backgroundImage = "url('images/paper-with-code.png')";
```

### Adjust Drop Zone Positions
In the `zones` array (around line 220), adjust the CSS positioning:
```javascript
{ id: 'zone-motor', accepts: 'motor', label: 'Motor', style: 'top: 20%; left: 15%; width: 25%; height: 20%;' }
```

## Integration with Main Game

To call this minigame from your main game:
```javascript
// Simple call
showLaundryMinigame();

// With callback when complete
showLaundryMinigame(() => {
    // Your code here - unlock door, add code to inventory, etc.
    console.log('Player found the code!');
    addCodeToInventory('4729');
    unlockBasementDoor();
});
```

## Troubleshooting

### Images Not Loading
- Check that image paths include `images/` prefix
- Verify all images exist in the `images/` folder
- Use browser DevTools (F12) → Console to see errors

### Drag and Drop Not Working
- Make sure you're running via a server (not just file://)
- Check browser console for JavaScript errors

### Code Not Displaying
- Verify the paper discovery section is present in the JavaScript
- Check that the wash cycle completes successfully

## Credits
- Game Design: [Your Name]
- Code: Laundry Minigame System
- Assets: [Your asset sources]

## License
[Your license here]