# React Dodgy Game - API Documentation

## Table of Contents

- [Overview](#overview)
- [Components](#components)
  - [Board](#board)
  - [DebugState](#debugstate)
  - [Enemy](#enemy)
  - [GameInfo](#gameinfo)
  - [Player](#player)
  - [Square](#square)
- [Containers](#containers)
  - [Game](#game)
- [Helpers](#helpers)
  - [Constants](#constants)
  - [Utils](#utils)
- [Getting Started](#getting-started)
- [Usage Examples](#usage-examples)

## Overview

The React Dodgy Game is an infinite dodging game built with React. Players control a character using arrow keys to avoid enemies that spawn from the edges of the game board. The game increases in difficulty over time by adding more enemies and increasing their speed.

## Components

### Board

A container component that creates the game board with a fixed dimension and styled border.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `dimension` | `number` | Yes | The width and height of the board in pixels |
| `children` | `ReactNode` | No | Child components to render inside the board |

#### Usage

```jsx
import { Board } from 'components';

<Board dimension={275}>
  {/* Game entities go here */}
</Board>
```

#### Styling

The board automatically applies:
- Dashed border (1px solid black)
- Centered positioning with 25px top/bottom margin
- Hidden overflow
- 8px border radius
- Relative positioning for absolute-positioned children

---

### DebugState

A debugging component that displays the current game state as formatted JSON.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `data` | `object` | Yes | The state object to display |

#### Usage

```jsx
import { DebugState } from 'components';

<DebugState data={this.state} />
```

#### Features

- Fixed positioning at top-left corner (top: 0, left: 16px)
- Pretty-printed JSON with 2-space indentation
- Useful for development and debugging

---

### Enemy

A game entity that moves toward the player and detects collisions.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `size` | `number` | Yes | The size of the enemy square in pixels |
| `info` | `object` | Yes | Enemy position and direction info |
| `info.top` | `number` | Yes | Y-position in pixels |
| `info.left` | `number` | Yes | X-position in pixels |
| `info.dir` | `string` | Yes | Movement direction (UP, DOWN, LEFT, RIGHT) |
| `playerPosition` | `object` | Yes | Current player position for collision detection |
| `playerPosition.top` | `number` | Yes | Player Y-position |
| `playerPosition.left` | `number` | Yes | Player X-position |
| `onCollide` | `function` | Yes | Callback function when collision occurs |

#### Usage

```jsx
import { Enemy } from 'components';

<Enemy
  size={25}
  info={{
    top: 100,
    left: 150,
    dir: 'DOWN'
  }}
  playerPosition={{
    top: 125,
    left: 125
  }}
  onCollide={() => console.log('Collision detected!')}
/>
```

#### Collision Detection

The component uses `componentDidUpdate()` to check for collisions with the player using bounding box collision detection. When a collision is detected, the `onCollide` callback is triggered.

---

### GameInfo

Displays game statistics and instructions to the player.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `timeElapsed` | `number` | Yes | - | Time elapsed in seconds |
| `playerScore` | `number` | Yes | - | Current player score |
| `highScore` | `number` | Yes | - | Session high score |
| `globalHighScore` | `number|string` | No | `'Loading...'` | Global high score across all players |

#### Usage

```jsx
import { GameInfo } from 'components';

<GameInfo
  timeElapsed={45}
  playerScore={450}
  highScore={1200}
  globalHighScore={5000}
/>
```

#### Layout

- Centered text alignment
- Instructions: "Use arrows to move."
- Two rows of statistics in flex layout
- Row 1: Time and Score
- Row 2: High Score and Global High Score

---

### Player

The player-controlled character that responds to keyboard input.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `size` | `number` | Yes | The size of the player square in pixels |
| `position` | `object` | Yes | Current position |
| `position.top` | `number` | Yes | Y-position in pixels |
| `position.left` | `number` | Yes | X-position in pixels |
| `handlePlayerMovement` | `function` | Yes | Callback for movement input |

#### Usage

```jsx
import { Player } from 'components';

<Player
  size={25}
  position={{ top: 125, left: 125 }}
  handlePlayerMovement={(direction) => {
    // direction = { top: number, left: number, dir: string }
    console.log('Player moving:', direction);
  }}
/>
```

#### Keyboard Controls

| Key | Direction | Movement Object |
|-----|-----------|-----------------|
| ←  | LEFT | `{ top: 0, left: -1, dir: 'LEFT' }` |
| ↑  | UP | `{ top: -1, left: 0, dir: 'UP' }` |
| →  | RIGHT | `{ top: 0, left: 1, dir: 'RIGHT' }` |
| ↓  | DOWN | `{ top: 1, left: 0, dir: 'DOWN' }` |

#### Lifecycle

- `componentDidMount()`: Attaches global keydown event listener
- The player appears as a pink/red square (#E62E66)

---

### Square

A basic styled square component used by Player and Enemy components.

#### Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `size` | `number` | Yes | Width and height in pixels |
| `position` | `object` | Yes | Absolute position |
| `position.top` | `number` | Yes | Y-position in pixels |
| `position.left` | `number` | Yes | X-position in pixels |
| `color` | `string` | Yes | Background color (CSS color value) |

#### Usage

```jsx
import { Square } from 'components';

<Square
  size={25}
  position={{ top: 100, left: 100 }}
  color="#FF0000"
/>
```

#### Styling

- Absolute positioning
- Smooth transitions (0.1s ease)
- 8px border radius
- Square dimensions based on `size` prop

## Containers

### Game

The main game container that manages all game state, logic, and rendering.

#### Props

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `boardSize` | `number` | Yes | - | Grid size (e.g., 11 creates 11x11 grid) |
| `playerSize` | `number` | Yes | - | Size of player and enemies in pixels |

#### Usage

```jsx
import { Game } from 'containers';
import { render } from 'react-dom';

render(
  <Game boardSize={11} playerSize={25} />,
  document.getElementById('root')
);
```

#### State Structure

```javascript
{
  size: {
    board: number,        // Grid size
    player: number,       // Player/enemy size in pixels
    maxDim: number        // Total board dimension in pixels
  },
  positions: {
    player: {
      top: number,        // Player Y position
      left: number        // Player X position
    },
    enemies: [            // Array of enemy objects
      {
        key: number,      // Unique identifier
        top: number,      // Y position
        left: number,     // X position
        dir: string,      // Direction (UP/DOWN/LEFT/RIGHT)
        remove?: boolean  // Flag for removal
      }
    ]
  },
  playerScore: number,    // Current score
  highScore: number,      // Session high score
  globalHighScore: number,// Global high score
  timeElapsed: number,    // Time in seconds
  enemySpeed: number,     // Enemy movement speed
  enemyIndex: number,     // Counter for unique enemy keys
  activeEnemies: number,  // Max concurrent enemies
  baseScore: number,      // Points per second
  debug?: boolean         // Debug mode flag
}
```

#### Public Methods

##### `handlePlayerMovement(dirObj)`

Processes player movement input and updates player position.

**Parameters:**
- `dirObj` (object): Movement direction object
  - `top` (number): Vertical movement (-1, 0, or 1)
  - `left` (number): Horizontal movement (-1, 0, or 1)
  - `dir` (string): Direction constant

**Example:**
```javascript
// Called automatically by Player component
game.handlePlayerMovement({ top: 0, left: 1, dir: 'RIGHT' });
```

##### `handlePlayerCollision()`

Handles collision between player and enemy, triggering game reset.

**Example:**
```javascript
// Called automatically by Enemy component
game.handlePlayerCollision();
```

#### Game Mechanics

##### Difficulty Progression

- **Enemy Speed**: Increases by 0.25 every 3 seconds
- **Enemy Count**: Increases by 1 every 10 seconds
- **Scoring**: 10 points per second survived

##### Enemy Behavior

- Enemies spawn from random board edges
- Always aim directly at player's current position
- Move at constant speed toward player
- Removed when they exit the board boundaries

##### Collision Detection

- Uses bounding box collision detection
- Collision triggers immediate game reset
- High score is preserved between games

#### Lifecycle Methods

##### `componentDidMount()`

- Starts game intervals
- Attempts to fetch global high score (if API configured)

##### `componentWillUnmount()`

- Cleans up all game intervals
- Prevents memory leaks

#### Game Intervals

The game uses three intervals for different update cycles:

- **Enemy Position Update**: 50ms - Updates enemy positions and removes off-screen enemies
- **Game Logic Update**: 1000ms - Updates time, score, difficulty progression
- **Enemy Spawning**: 250ms - Manages enemy spawning based on active enemy count

## Helpers

### Constants

Direction constants for consistent movement handling.

#### Exports

```javascript
export const UP = 'UP';
export const DOWN = 'DOWN';
export const LEFT = 'LEFT';
export const RIGHT = 'RIGHT';
```

#### Usage

```javascript
import { UP, DOWN, LEFT, RIGHT } from 'helpers/constants';

// Use in switch statements
switch (direction) {
  case UP:
    // Handle upward movement
    break;
  case DOWN:
    // Handle downward movement
    break;
  // etc.
}
```

---

### Utils

Utility functions for common operations.

#### `puke(obj)`

Renders an object as formatted JSON for debugging purposes.

**Parameters:**
- `obj` (any): Object to display

**Returns:**
- `ReactElement`: Pre-formatted JSX element

**Usage:**
```javascript
import { puke } from 'helpers/utils';

const data = { score: 100, level: 5 };
return <div>{puke(data)}</div>;
```

#### `pluck(arr)`

Selects a random element from an array.

**Parameters:**
- `arr` (Array): Array to select from

**Returns:**
- `any`: Random element from the array

**Usage:**
```javascript
import { pluck } from 'helpers/utils';

const directions = ['UP', 'DOWN', 'LEFT', 'RIGHT'];
const randomDirection = pluck(directions);
console.log(randomDirection); // e.g., 'LEFT'
```

## Getting Started

### Installation

1. Clone the repository
2. Install dependencies:
   ```bash
   yarn install
   # or
   npm install
   ```

### Development

Start the development server:
```bash
yarn start
# or
npm start
```

### Production Build

Create a production build:
```bash
yarn build
# or
npm run build
```

## Usage Examples

### Basic Game Setup

```jsx
import React from 'react';
import { render } from 'react-dom';
import { Game } from 'containers';

// Create a game with 11x11 grid and 25px entities
render(
  <Game boardSize={11} playerSize={25} />,
  document.getElementById('root')
);
```

### Custom Component Usage

```jsx
import React from 'react';
import { Board, Player, Enemy, Square } from 'components';
import { UP } from 'helpers/constants';

function CustomGameBoard() {
  return (
    <Board dimension={300}>
      <Player
        size={20}
        position={{ top: 140, left: 140 }}
        handlePlayerMovement={(dir) => console.log(dir)}
      />
      
      <Enemy
        size={20}
        info={{ top: 0, left: 140, dir: UP }}
        playerPosition={{ top: 140, left: 140 }}
        onCollide={() => alert('Game Over!')}
      />
      
      <Square
        size={15}
        position={{ top: 50, left: 50 }}
        color="#00FF00"
      />
    </Board>
  );
}
```

### Using Utility Functions

```jsx
import { pluck, puke } from 'helpers/utils';
import { UP, DOWN, LEFT, RIGHT } from 'helpers/constants';

// Random direction selection
const directions = [UP, DOWN, LEFT, RIGHT];
const randomDirection = pluck(directions);

// Debug state display
function DebugPanel({ gameState }) {
  return (
    <div>
      <h3>Game State:</h3>
      {puke(gameState)}
    </div>
  );
}
```

### Extending the Game

```jsx
import React, { Component } from 'react';
import { Game } from 'containers';

class CustomGame extends Component {
  render() {
    return (
      <div>
        <h1>My Custom Dodgy Game</h1>
        <Game boardSize={15} playerSize={20} />
        <p>Use arrow keys to dodge the enemies!</p>
      </div>
    );
  }
}
```

---

*This documentation covers version 0.0.1 of the React Dodgy Game. For the latest updates and issues, visit the project repository.*