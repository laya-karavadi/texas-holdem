# Poker Algorithm - Texas Hold'em AI

A Python implementation of a Texas Hold'em poker AI that uses hand evaluation, pot odds calculation, and opponent modeling to make strategic betting decisions.

## 🃏 Features

### Core Functionality

- **Complete Hand Evaluation**: Recognizes all poker hands from High Card to Royal Flush
- **Pot Odds Calculation**: Makes mathematically sound decisions based on pot odds
- **Opponent Modeling**: Tracks opponent betting patterns and adjusts strategy accordingly
- **Adaptive Aggression**: Dynamically adjusts aggression based on opponent behavior
- **Bluffing Logic**: Incorporates calculated bluffing into decision-making

### Hand Rankings Supported

1. **Royal Flush** (10-J-Q-K-A suited)
2. **Straight Flush** (5 consecutive cards, same suit)
3. **Four of a Kind** (4 cards of same rank)
4. **Full House** (3 of a kind + pair)
5. **Flush** (5 cards of same suit)
6. **Straight** (5 consecutive cards)
7. **Three of a Kind** (3 cards of same rank)
8. **Two Pair** (2 pairs of different ranks)
9. **Pair** (2 cards of same rank)
10. **High Card** (no other combination)

## 🚀 Quick Start

### Prerequisites

- Python 3.7+
- No additional packages required (uses only standard library)

### Installation

1. Download the `poker_algorithm.py` file
2. Run directly:
   ```bash
   python poker_algorithm.py
   ```
   or
   ```bash
   python3 poker_algorithm.py
   ```

### Basic Usage

```python
from poker_algorithm import PokerAlgorithm

# Initialize the algorithm
ai_player = PokerAlgorithm()

# Example game state
game_state = {
    'hole_cards': [('A', 'H'), ('K', 'S')],
    'community_cards': [('Q', 'D'), ('J', 'C'), ('10', 'H')],
    'current_bet': 100,
    'chips': 1500,
    'pot': 300,
    'opponent_chips': 1200,
    'phase': 'flop'
}

# Get AI decision
action, amount = ai_player.make_decision(game_state)
print(f"AI Decision: {action} {amount}")
```

## 🎮 Interactive Game Mode

The program includes a full interactive mode where you can play against the AI:

### Starting a Game

```bash
python poker_algorithm.py
```

### Game Flow

1. **Setup**: Enter starting chip count
2. **Hole Cards**: Input your two hole cards (e.g., "Ah Ks")
3. **Turn Order**: Choose who acts first (0 for AI, 1 for opponent)
4. **Betting Rounds**: Play through preflop, flop, turn, and river
5. **Showdown**: Compare hands if no one folds

### Card Input Format

- **Values**: 2, 3, 4, 5, 6, 7, 8, 9, 10, J, Q, K, A
- **Suits**: H (Hearts), D (Diamonds), C (Clubs), S (Spades)
- **Examples**: Ah (Ace of Hearts), Ks (King of Spades), 10d (Ten of Diamonds)

### Betting Actions

- **Check**: No bet when no one has bet yet
- **Call**: Match the current bet
- **Bet [amount]**: Make the first bet (e.g., bet 100)
- **Raise [amount]**: Increase the bet (e.g., raise 200)
- **Fold**: Give up the hand

## 🧠 Algorithm Strategy

### Hand Strength Evaluation

The AI calculates hand strength on a scale of 0.05 to 0.95:

- **Premium hands** (A-A, A-K): +15% bonus
- **Strong hands** (Any Ace, high pairs): +10% bonus
- **Potential improvement**: Adjusts for remaining community cards

### Decision Making Logic

#### When No Bet Exists (Check/Bet)

- **Strong hands** (>0.7): Aggressive betting (~75% of pot)
- **Medium hands** (0.4-0.7): Occasional betting based on aggression
- **Weak hands** (<0.4): Rare bluffs (~20% chance)

#### When Facing a Bet (Call/Raise/Fold)

- **Above pot odds + margin**: Call or raise
- **Near pot odds**: Call
- **Below pot odds**: Fold (with occasional bluffs)

### Opponent Modeling

The AI tracks opponent patterns across all betting rounds:

- **Aggressive opponents**: Reduces own aggression factor
- **Passive opponents**: Increases aggression factor
- **Pattern recognition**: Learns from betting history

## 📊 Technical Details

### Core Classes and Methods

#### PokerAlgorithm

```python
# Main methods
evaluate_hand(hole_cards, community_cards)    # Returns hand type string
calculate_hand_strength(hole_cards, community_cards)  # Returns 0-1 strength
calculate_pot_odds(game_state)                # Returns pot odds ratio
make_decision(game_state)                     # Returns (action, amount)
update_opponent_model(phase, action, amount)  # Updates opponent tracking
```

### Hand Evaluation Algorithm

- **Flush detection**: Counts suits to find 5+ of same suit
- **Straight detection**: Checks for 5 consecutive values (handles A-5 straight)
- **Pair/trips detection**: Uses Counter to find duplicates
- **Combination logic**: Prioritizes higher-ranking hands

### Pot Odds Calculation

```
Pot Odds = Current Pot / (Current Pot + Call Amount)
```

## 🎯 Strategy Tips

### Playing Against the AI

- **Early aggression**: AI adapts to aggressive opponents by becoming more cautious
- **Tight play**: AI will increase aggression against passive opponents
- **Mixed strategies**: Vary your play to prevent pattern recognition

### Customizing the AI

```python
# Adjust aggression levels
ai_player.aggression = 0.8  # More aggressive (0.0 - 1.0)

# Modify hand strength thresholds in make_decision()
if hand_strength > 0.6:  # Lower threshold for betting
    # More aggressive play

# Adjust pot odds margins
if hand_strength > pot_odds + 0.05:  # Smaller margin
    # More willing to call
```

## 🔧 Advanced Configuration

### Bluffing Parameters

- **Strong hand bluff rate**: 0% (value betting only)
- **Medium hand bluff rate**: Variable based on aggression
- **Weak hand bluff rate**: 10-20% of the time

### Bet Sizing

- **Strong hands**: 75% of pot
- **Medium hands**: 40% of pot
- **Bluffs**: 30% of pot

### Aggression Adaptation

- **Range**: 0.3 to 0.8
- **Adjustment**: ±0.1 based on opponent actions
- **Default**: 0.5 (balanced)

## 🐛 Troubleshooting

### Common Issues

- **Invalid card input**: Ensure format is correct (e.g., "Ah", "10s")
- **Betting amount errors**: Check minimum bets and available chips
- **Action sequence errors**: Follow proper betting order (can't bet after someone bets, must raise)

### Debug Information

The AI prints decision details:

```
Hand strength: 0.65
Phase: flop
Pot: 200, Current bet: 50
Your action: CALL 50
```

## 📈 Performance Metrics

### Hand Evaluation Accuracy

- Correctly identifies all standard poker hands
- Handles edge cases (A-5 straight, wheel straight flush)
- Processes both hole cards and community cards

### Decision Speed

- Instant evaluation for most game states
- Efficient Counter-based hand analysis
- Minimal memory usage for opponent modeling

## 🤝 Contributing

### Potential Improvements

- **Position awareness**: Factor in betting position
- **Stack-to-pot ratio**: Adjust strategy based on stack sizes
- **Multi-opponent support**: Handle games with 3+ players
- **Advanced opponent modeling**: Machine learning for pattern recognition
- **Tournament strategy**: Adjust for blinds and bubble play

### Code Structure

```
poker_algorithm.py
├── PokerAlgorithm class
│   ├── Hand evaluation methods
│   ├── Decision making logic
│   └── Opponent modeling
└── Interactive game loop
    ├── Input handling
    ├── Game state management
    └── Showdown logic
```

## 📚 Learning Resources

### Poker Theory

- **Pot odds**: Understanding mathematical advantage
- **Position play**: Importance of acting last
- **Bankroll management**: Risk management strategies
- **Tournament vs. cash game**: Different strategic approaches

### Algorithm Concepts

- **Game theory**: Nash equilibrium in poker
- **Monte Carlo simulation**: Estimating hand equity
- **Neural networks**: Advanced opponent modeling
- **Reinforcement learning**: Self-improving poker AI

## 🏆 Example Game Session

```
Starting chips: 1000

--- PREFLOP ---
Your cards: Ah Ks
You are first to act
Hand strength: 0.85
Phase: preflop
Pot: 1, Current bet: 0
Your action: BET 75

Opponent action: call 75

--- FLOP ---
Enter flop card 1: Qd
Enter flop card 2: Jc  
Enter flop card 3: 10h
Your cards: Ah Ks
Community cards: Qd Jc 10h
Hand strength: 0.95
Your action: BET 150

--- SHOWDOWN ---
Your hand: Straight
Opponent hand: Pair
You win the pot!
```

---

This poker AI provides a solid foundation for understanding poker strategy and can serve as a training opponent or a starting point for more advanced poker AI development.
