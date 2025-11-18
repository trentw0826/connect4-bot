# Connect 4 AI Bot

## Overview

This project demonstrates understanding of adversarial search techniques using a Connect 4. The program interact with a human agent, a random AI agent, or an intelligent agent

## Features

### Implemented (Part 2)
- **Alpha-Beta Pruning Minimax**: A depth-limited minimax search with alpha-beta pruning for optimal move selection
- **Game Engine**: Complete Connect 4 game implementation with board representation and win detection
- **Multiple Player Types**: Human, Random, and AI players
- **Configurable Depth**: Adjustable search depth for alpha-beta algorithm
- **Performance Testing**: Automated game testing between different player types

### Planned
- **Expectimax Algorithm**: For playing against probabilistic opponents
- **Monte Carlo Tree Search (MCTS)**: Advanced AI technique using random simulations

## Requirements

- Python 3.x
- NumPy
- tkinter (optional, for GUI - automatically disabled if not available)

## Installation

1. Clone the repository
2. Install dependencies:
   ```bash
   pip install numpy
   ```

## Usage

### Basic Game Command
```bash
python ConnectFour.py <player1_type> <player2_type> [options]
```

### Player Types
- `ab`: Alpha-beta pruning AI
- `random`: Random move player
- `human`: Human player (interactive)
- `mcts`: Monte Carlo Tree Search (not yet implemented)
- `expmax`: Expectimax (not yet implemented)

### Examples

**AI vs Random Player (single game):**
```bash
python ConnectFour.py ab random -n 1
```

**AI vs AI with different depths:**
```bash
python ConnectFour.py ab ab -p1 3 -p2 5 -n 5
```

**Human vs AI:**
```bash
python ConnectFour.py human ab -n 1
```

### Command Line Options

- `-n, --number`: Number of games each player goes first (default: 1)
- `-p1, --params1`: Parameters for player 1 (e.g., depth limit for alpha-beta)
- `-p2, --params2`: Parameters for player 2  
- `-t, --time`: Time limit per move in seconds (default: 60)

### Alpha-Beta Configuration

The alpha-beta player accepts a depth limit parameter:
```bash
python ConnectFour.py ab random -p1 5  # Alpha-beta with depth limit 5
```

## AI Implementation Details

### Evaluation Function (Task 2.1.1)
The evaluation function breaks down the board into a set of 'windows' of length 4 (each vertical, horizontal, and diagonal length that a player could fill to win). The function applies and sums a simple sub-heuristic applied to each window which detects how 'good' or 'bad' the window is for the current player. In other words, how far away is the player from winning or losing based on each individual window, summed across every window.

### Alpha-Beta Minimax
- **Algorithm**: Minimax with alpha-beta pruning
- **Depth Limit**: Configurable (default: 3)
- **Evaluation Function**: Considers center control, potential wins, and opponent threats
- **Move Ordering**: No specific ordering implemented
- **Terminal Detection**: Checks for wins, losses, and ties
- **Pruning**: Standard alpha-beta cutoffs to reduce search space

The alpha-beta implementation includes:
- Proper handling of terminal states (wins/losses/ties)
- Depth-adjusted scoring (prefers winning sooner, losing later)
- Robust evaluation function that avoids errors on winning positions
- Efficient pruning to reduce computational complexity

## AI Performance Analysis

### Alpha-Beta Minimax (Task 2.1.2 - 2.1.6)

Time taken for initial move - All depths landed on column 3, decision time grew exponentially with depth
Depth | First Move Selected | Time (seconds)
------|---------------|---------------
  1   |       3       |     0.0035
  2   |       3       |     0.0105  
  3   |       3       |     0.0705
  4   |       3       |     0.1716
  5   |       3       |     0.8955

Efficiency against me - I could only consistently win against the algorithm at depth 2. At depth 3 there were ties and a few losses on my end, and by depth 5 I was consistently losing

Efficiency against random agent - AB algorithm easily beats the random agent at all depths (though the random agent can still get lucky)
Depth | Wins | Ties | Losses | Win Rate
------|------|------|--------|----------
  1   |  20  |   0  |    0   |  100.0%
  2   |  20  |   0  |    0   |  100.0%
  3   |  19  |   0  |    1   |   95.0%
  4   |  20  |   0  |    0   |  100.0%
  5   |  20  |   0  |    0   |  100.0%

Depth v Depth combinations - Though depth 4 and 5 generally beat other depths, the algorithms tie at depth 3 versus depth 5 may suggest that a more informed heuristic or wider depth difference would make a more meaningful difference between them
Matchup     | Player 1 (W-T-L) | Player 2 (W-T-L) | Winner
------------|-------------------|-------------------|--------
Depth 1 vs 2|  5- 0- 5         |  5- 0- 5         | Tie
Depth 1 vs 3|  5- 0- 5         |  5- 0- 5         | Tie  
Depth 1 vs 4|  0- 0-10         | 10- 0- 0         | Depth 4
Depth 2 vs 3|  5- 0- 5         |  5- 0- 5         | Tie
Depth 2 vs 4|  0- 0-10         | 10- 0- 0         | Depth 4
Depth 2 vs 5|  5- 0- 5         |  5- 0- 5         | Tie
Depth 3 vs 4|  0- 0-10         | 10- 0- 0         | Depth 4
Depth 3 vs 5|  0- 0-10         | 10- 0- 0         | Depth 5
Depth 4 vs 5|  5- 0- 5         |  5- 0- 5         | Tie

First player advantage - Based on the depth v depth results, there was no meaningful difference between the first and second agent's play when depths were the same. Existing understanding of the game shows that Connect 4 is solved and is a win for the first player, suggesting that the AB algorithm is not running deep enough to uncover this pattern.


## Implementation Reflection

### Alpha-Beta Minimax (Task 2.1.7)
The most difficult part of this implementation was cleanly designing the alpha-beta function to properly prune branches. Initially, I attempted to do a form of a recursive check on the bounds of the parent states and quickly realized that even if I could get it working, the steps required would be too heavy. After some research, I landed on an iterative bound-tightening method that feels reminiscent of binary search, tightening the alpha-beta parameters of states on an individual level until they proved any further exploration along that branch to be inefficient. If I were to start from scratch again, I would start by researching actual implementations of the minimax algorithm for simple games to get a better handle on how such programs are run in practice. Overall, this assignment took about 2-3 hours of work.


## AI Disclosure

README generated in part by Claude Sonnet 4.0

