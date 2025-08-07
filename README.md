# React Dodgy Game

An infinite dodging game built with React where players control a character using arrow keys to avoid enemies that spawn from the edges of the game board.

## 🎮 Game Features

- **Infinite Gameplay**: Survive as long as possible while dodging enemies
- **Progressive Difficulty**: Enemy speed and count increase over time
- **Score Tracking**: Local high score persistence with global leaderboard support
- **Responsive Controls**: Smooth keyboard-based movement with arrow keys
- **Modern UI**: Clean, minimalist design with smooth animations

## 🚀 Quick Start

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd react-dodgy-game

# Install dependencies
yarn install
# or
npm install
```

### Development

```bash
# Start development server
yarn start
# or
npm start
```

The game will open in your browser at `http://localhost:8080`.

### Production Build

```bash
# Create production build
yarn build
# or
npm run build
```

## 🕹️ How to Play

1. **Movement**: Use arrow keys (↑ ↓ ← →) to move your character
2. **Objective**: Avoid the gray enemy squares that spawn from the board edges
3. **Scoring**: Earn 10 points per second survived
4. **Difficulty**: Every 3 seconds enemies get faster, every 10 seconds more enemies spawn
5. **Game Over**: Touching an enemy resets the game but preserves your high score

## 🏗️ Architecture

The game is built using React with a component-based architecture:

### Components
- **Board**: Game container with styled boundaries
- **Player**: Player-controlled character with keyboard input handling
- **Enemy**: AI-controlled enemies with collision detection
- **GameInfo**: Displays score, time, and game statistics
- **Square**: Base visual component for game entities
- **DebugState**: Development tool for state inspection

### Containers
- **Game**: Main game logic, state management, and orchestration

### Helpers
- **Constants**: Direction enums for consistent movement handling
- **Utils**: Utility functions for debugging and random selection

## 📚 Documentation

For complete API documentation, component details, and usage examples, see:

**[📖 API Documentation](./API_DOCUMENTATION.md)**

The documentation includes:
- Detailed component APIs with props and examples
- Game state structure and methods
- Helper function references
- Usage patterns and best practices
- Extension examples

## 🎯 Game Mechanics

### Difficulty Progression
- **Enemy Speed**: Increases by 0.25 units every 3 seconds
- **Enemy Count**: Increases by 1 enemy every 10 seconds  
- **Scoring**: 10 points per second + survival time bonus

### Technical Details
- **Grid System**: Configurable board size (default: 11x11)
- **Entity Size**: Configurable player/enemy size (default: 25px)
- **Update Cycles**: 
  - Enemy movement: 50ms intervals
  - Game logic: 1000ms intervals  
  - Enemy spawning: 250ms intervals

## 🛠️ Development

### Project Structure

```
src/
├── components/          # Reusable React components
│   ├── Board.jsx
│   ├── Player.jsx
│   ├── Enemy.jsx
│   ├── GameInfo.jsx
│   ├── Square.jsx
│   ├── DebugState.jsx
│   └── index.js
├── containers/          # Container components
│   ├── Game.jsx
│   └── index.js
├── helpers/            # Utility functions and constants
│   ├── constants.js
│   └── utils.js
└── index.js            # Application entry point
```

### Key Technologies
- **React 15.1.0**: UI library
- **Webpack**: Module bundling and development server
- **Babel**: ES6+ transpilation
- **Axios**: HTTP client (for global leaderboard features)

### Debug Mode

Enable debug mode to inspect game state in real-time:

```jsx
// In Game.jsx, change line 325 from:
{false && <p style={{ position: 'fixed', bottom: 0, left: 16 }}>Debug: <input type="checkbox" onChange={this.handleDebugToggle} ref={ n => this.debug = n }/></p>}

// To:
{true && <p style={{ position: 'fixed', bottom: 0, left: 16 }}>Debug: <input type="checkbox" onChange={this.handleDebugToggle} ref={ n => this.debug = n }/></p>}
```

## 🎨 Customization

### Game Configuration

Modify the game parameters in `src/index.js`:

```jsx
render(
    <Game 
        boardSize={11}      // Grid size (11x11)
        playerSize={25}     // Entity size in pixels
    />
, root);
```

### Styling

Component styles are defined inline. Key style configurations:

- **Board**: Dashed border, centered layout, hidden overflow
- **Player**: Pink/red color (#E62E66)
- **Enemy**: Gray color (#5A5A5A)
- **Transitions**: 0.1s ease for smooth movement

### Adding Features

The modular architecture makes it easy to extend:

1. **New Game Modes**: Extend the Game container
2. **Visual Effects**: Add components or modify Square styling  
3. **Power-ups**: Create new components and integrate with game logic
4. **Sound**: Add audio hooks to game events

## 🌐 Global Leaderboard

The game includes infrastructure for a global leaderboard (currently disabled):

```javascript
// Uncomment in Game.jsx to enable API integration
// import url from 'api';
```

To implement:
1. Set up a backend API endpoint
2. Configure the API URL
3. Uncomment the axios calls in `fetchGlobalHighScore()` and `updateGlobalHighScore()`

## 📄 License

ISC License - see package.json for details.

## 👨‍💻 Author

Brandon Stilson - [brandonstilson.com](http://brandonstilson.com)

---

**[🎮 Play the Game](http://brandonstilson.com/dodgygame)** | **[📖 Full API Documentation](./API_DOCUMENTATION.md)**