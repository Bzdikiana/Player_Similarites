# ⚽ Player Similarity Model

> **Advanced Context-Aware Player Similarity Analysis using StatsBomb Data**

A comprehensive machine learning pipeline for computing explainable, multi-dimensional player similarity in football/soccer. This system combines behavioral embeddings, context-aware analysis, graph neural networks, and interpretable feature breakdowns.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![StatsBomb](https://img.shields.io/badge/Data-StatsBomb-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Detailed Documentation](#detailed-documentation)
  - [Part 1: Data Loading & Preprocessing](#part-1-data-loading--preprocessing)
  - [Part 2: Behavioral Feature Extraction](#part-2-behavioral-feature-extraction)
  - [Part 3: Context-Aware Similarity](#part-3-context-aware-similarity)
  - [Part 4: Explainable Similarity](#part-4-explainable-similarity)
- [Mathematical Foundations](#mathematical-foundations)
- [Feature Engineering](#feature-engineering)
- [Future Improvements](#future-improvements)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

This project provides a sophisticated player similarity system that answers not just "**who is similar?**" but also "**why are they similar?**" and "**in what context?**"

### Key Capabilities

| Capability | Description |
|------------|-------------|
| **Multi-dimensional Similarity** | Compares players across 7+ behavioral dimensions |
| **Context-Aware** | Adapts to game state (winning/losing/drawing), game phase, competition type |
| **Explainable** | Shows exactly which features drive similarity |
| **Role Discovery** | Automatically discovers tactical roles using clustering |
| **Spatial Analysis** | Uses 360° freeze-frame data for spatial embeddings |

---

## ✨ Features

### 🎨 Behavioral Style Analysis
- **Spatial Patterns**: Where players operate on the pitch
- **Passing Style**: Length distribution, direction preference, progressive passes
- **Receiving Patterns**: How and where players receive the ball
- **Pressing Behavior**: Defensive work rate and pressing locations
- **Transition Play**: Counter-attacking patterns and ball recoveries
- **Value Contribution**: Expected Threat (xT) generation

### 🌍 Context-Aware Similarity
- **Score State**: Behavior when winning, losing, or drawing
- **Game Phase**: Early game, mid-game, closing, injury time
- **Competition Type**: League vs. Cup differences
- **Spatial Context**: Pressure situations using 360° data

### 📊 Explainability
- Feature-by-feature similarity breakdown
- Top drivers of similarity
- Key differentiators between players
- Visual dashboards and radar charts

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    PLAYER SIMILARITY PIPELINE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────────┐   │
│  │ StatsBomb    │───▶│ Pitch        │───▶│ Feature          │   │
│  │ Events API   │    │ Normalizer   │    │ Extractor        │   │
│  └──────────────┘    └──────────────┘    └──────────────────┘   │
│                                                  │               │
│                                                  ▼               │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────────┐   │
│  │ 360 Freeze   │───▶│ Spatial      │───▶│ Player           │   │
│  │ Frame Data   │    │ Embedder     │    │ Profiles         │   │
│  └──────────────┘    └──────────────┘    └──────────────────┘   │
│                                                  │               │
│                                                  ▼               │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────────┐   │
│  │ Context      │───▶│ Scenario     │───▶│ Context-Aware    │   │
│  │ (Score/Phase)│    │ Embedder     │    │ Embeddings       │   │
│  └──────────────┘    └──────────────┘    └──────────────────┘   │
│                                                  │               │
│                                                  ▼               │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────────┐   │
│  │ Team Graph   │───▶│ GNN Layer    │───▶│ Unified          │   │
│  │ Builder      │    │ (2-layer GCN)│    │ Similarity       │   │
│  └──────────────┘    └──────────────┘    └──────────────────┘   │
│                                                  │               │
│                                                  ▼               │
│                              ┌──────────────────────────────┐   │
│                              │  EXPLAINABLE SIMILARITY API  │   │
│                              │  • Compare players           │   │
│                              │  • Find similar players      │   │
│                              │  • Feature breakdown         │   │
│                              │  • Visualizations            │   │
│                              └──────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/player-similarity-model.git
cd player-similarity-model

# Create virtual environment
python -m venv .venv

# Activate virtual environment
# Windows:
.venv\Scripts\activate
# Linux/Mac:
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

---

## 🎮 Quick Start

```python
# Import and run the notebook
# Or use the API directly:

from player_similarity import UnifiedExplainableSimilarityAPI

# Initialize (after running pipeline cells)
api = unified_api

# Compare two players
result = api.compare("Player A", "Player B")

# Print detailed report
api.print_full_report(result)

# Visualize
api.visualize(result)

# Find similar players
similar = api.find_similar("Player A", top_n=10)
for match in similar:
    print(f"{match['player2']}: {match['overall']:.1%}")
```

---

## 📚 Detailed Documentation

### Part 1: Data Loading & Preprocessing

#### Data Source: StatsBomb Open Data
We use StatsBomb's free open data which includes:
- **Competitions**: La Liga 2015/16, World Cup 2018, and more
- **Events**: Every on-ball action (passes, shots, carries, etc.)
- **360 Data**: Freeze-frame positions of all players

#### Pitch Normalization
All coordinates are normalized to a consistent attacking direction:

```python
class PitchNormalizer:
    """
    Normalizes all coordinates so players are always 
    attacking left-to-right regardless of actual direction.
    
    StatsBomb pitch dimensions: 120 x 80 yards
    """
    def normalize(self, x, y, attacking_right):
        if attacking_right:
            return 120 - x, 80 - y  # Flip coordinates
        return x, y
```

---

### Part 2: Behavioral Feature Extraction

#### 2.1 Expected Threat (xT) Model

The xT model assigns a value to each pitch location based on the probability of scoring from that position.

**Mathematical Definition:**

$$xT(x, y) = P(\text{shot}|x,y) \times P(\text{goal}|\text{shot}, x, y) + P(\text{move}|x,y) \times \sum_{z} P(z|x,y) \times xT(z)$$

Simplified empirical approach:

$$xT(x, y) = \frac{\text{Goals scored from zone}(x, y)}{\text{Actions in zone}(x, y)}$$

We divide the pitch into a 12×8 grid and compute xT for each zone:

```
        Goal
    ┌─────────┐
    │ 0.3 0.4 │  ← High xT near goal
    │ 0.2 0.2 │
    │ 0.1 0.1 │
    │ 0.05 0.05│
    │ 0.02 0.02│  ← Low xT in own half
    │ 0.01 0.01│
    └─────────┘
```

#### 2.2 Feature Blocks

| Feature Block | Dimensions | Description |
|--------------|------------|-------------|
| **Spatial** | 96 | Pitch divided into 12×8 zones, percentage of actions in each |
| **Passing** | ~50 | Pass length bins, angle bins, completion rates |
| **Receiving** | ~30 | Receive locations, under-pressure receives |
| **Pressing** | ~30 | Press locations, counter-press rate |
| **Transition** | ~20 | Ball recovery locations, carry distances |
| **Value Added** | ~20 | xT generated, progressive actions |
| **Action Chains** | ~165 | Common 3-action sequences (e.g., receive→dribble→pass) |

#### 2.3 Action Chain Encoding

We encode sequences of 3 consecutive actions:

```python
# Action vocabulary
actions = ['pass', 'carry', 'dribble', 'shot', 'press', 'receive', ...]

# Example chain: "receive → carry → pass"
chain_id = action_to_id['receive'] * 36 + action_to_id['carry'] * 6 + action_to_id['pass']

# Player profile includes frequency of each chain
profile['action_chains'][chain_id] += 1
```

---

### Part 3: Context-Aware Similarity

#### 3.1 Score State Embedding

Players behave differently based on the score:

```python
class ScoreState(Enum):
    WINNING_BIG = 0   # +2 or more goals
    WINNING = 1       # +1 goal
    DRAWING = 2       # 0-0 or tied
    LOSING = 3        # -1 goal
    LOSING_BIG = 4    # -2 or more goals
```

**Embedding**: One-hot encoding → 5 dimensions

#### 3.2 Game Phase Embedding

```python
class GamePhase(Enum):
    EARLY = 0        # 0-15 min
    BUILD_UP = 1     # 15-30 min
    FIRST_HALF_END = 2  # 30-45 min
    SECOND_START = 3    # 45-60 min
    PUSH = 4         # 60-75 min
    CLOSING = 5      # 75-85 min
    CRUCIAL = 6      # 85+ min
```

**Embedding**: One-hot encoding → 7 dimensions

#### 3.3 Spatial Embeddings from 360 Data

Using freeze-frame data, we compute:

```python
spatial_features = {
    'pressure_density': count_opponents_within_radius(5_yards),
    'passing_lanes': count_open_passing_options(),
    'space_ahead': measure_space_in_front(),
    'defensive_line_distance': distance_to_opponent_back_line(),
    ...
}
```

#### 3.4 Graph Neural Network (GNN)

We model team structure as a graph:

**Nodes**: Players
**Edges**: Passes between players

**Message Passing (2-layer GCN):**

$$h_i^{(l+1)} = \sigma\left(\sum_{j \in \mathcal{N}(i)} \frac{1}{\sqrt{d_i d_j}} W^{(l)} h_j^{(l)}\right)$$

Where:
- $h_i^{(l)}$ = embedding of player $i$ at layer $l$
- $\mathcal{N}(i)$ = neighbors of player $i$
- $d_i$ = degree of player $i$
- $W^{(l)}$ = learnable weight matrix

```python
class SimpleGNNLayer:
    """
    Graph Convolutional Layer
    
    Aggregates information from neighboring players
    (those who frequently pass to/receive from this player)
    """
    def forward(self, node_features, adjacency):
        # Normalize adjacency
        D = np.diag(adjacency.sum(axis=1) ** -0.5)
        A_norm = D @ adjacency @ D
        
        # Message passing
        messages = A_norm @ node_features @ self.weights
        
        return self.activation(messages)
```

---

### Part 4: Explainable Similarity

#### 4.1 Ratio-Based Similarity

Instead of cosine similarity, we use a ratio-based approach that handles sparse features better:

$$\text{sim}(a, b) = \frac{\min(a, b)}{\max(a, b)}$$

For vectors:

$$\text{BlockSim}(v_1, v_2) = \frac{1}{n} \sum_{i=1}^{n} \frac{\min(v_{1i}, v_{2i})}{\max(v_{1i}, v_{2i})}$$

**With Sigmoid Neutralization** (for rare actions):

$$\text{weight}_i = \sigma\left(\frac{\max(v_{1i}, v_{2i}) - \mu}{\sigma}\right)$$

This down-weights comparisons where both players rarely perform an action.

#### 4.2 Overall Similarity Computation

```python
overall_similarity = (
    0.50 * style_similarity +      # Behavioral patterns
    0.30 * context_similarity +    # Adaptation to game states
    0.20 * role_similarity         # Tactical role match
)
```

#### 4.3 Feature Contribution Analysis

For any player pair, we identify:

1. **Top Similar Features**: Which dimensions are most alike
2. **Top Different Features**: Which dimensions differ most
3. **Block-Level Breakdown**: Similarity per feature category

---

## 📐 Mathematical Foundations

### Embedding Space

Final player embedding:

$$e_{\text{player}} = \text{Concat}(e_{\text{base}}, e_{\text{spatial}}, e_{\text{scenario}}, e_{\text{gnn}})$$

Where:
- $e_{\text{base}} \in \mathbb{R}^{32}$ — PCA-reduced behavioral features
- $e_{\text{spatial}} \in \mathbb{R}^{13}$ — 360 spatial features
- $e_{\text{scenario}} \in \mathbb{R}^{22}$ — Context encoding
- $e_{\text{gnn}} \in \mathbb{R}^{32}$ — Team structure encoding

### Similarity Metrics

| Metric | Formula | Use Case |
|--------|---------|----------|
| Ratio-based | $\frac{\min(a,b)}{\max(a,b)}$ | Feature comparison |
| Cosine | $\frac{a \cdot b}{\|a\|\|b\|}$ | Embedding similarity |
| Weighted L2 | $\sum_i w_i (a_i - b_i)^2$ | Importance-weighted |

### Role Discovery

Using Gaussian Mixture Models:

$$P(z_k | x) = \frac{\pi_k \mathcal{N}(x|\mu_k, \Sigma_k)}{\sum_j \pi_j \mathcal{N}(x|\mu_j, \Sigma_j)}$$

Discovered roles: `inverted_fullback`, `deep_playmaker_6`, `box_to_box_8`, etc.

---

## 🔧 Feature Engineering

### Current Features (v1.0)

> ⚠️ **Note**: These features are initial implementations and will be refined in future versions.

| Category | Features | Status |
|----------|----------|--------|
| Spatial | 96-zone heatmap | ✅ Working |
| Passing | Length, angle, completion | ✅ Working |
| Receiving | Location, pressure | ✅ Working |
| Pressing | Location, frequency | ✅ Working |
| xT | Value generation | ✅ Working |
| Action Chains | 3-action sequences | ✅ Working |
| 360 Spatial | Pressure, lanes | ⚠️ Basic |
| GNN | 2-layer GCN | ⚠️ Basic |

### Planned Improvements

- [ ] Better spatial zone definitions (Voronoi-based)
- [ ] Velocity and acceleration features
- [ ] More sophisticated GNN architecture
- [ ] Attention mechanisms for context
- [ ] Time-series patterns
- [ ] Opposition-adjusted metrics

---

## 🔮 Future Improvements

### Short Term
- Refine feature weights based on domain expertise
- Add more granular passing features (through balls, switches)
- Improve action chain vocabulary

### Medium Term
- Implement attention-based context encoding
- Add player tracking data integration
- Build recommendation system for transfers

### Long Term
- Video-based feature extraction
- Real-time similarity updates
- Multi-season trend analysis

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **StatsBomb** for providing free open data
- **MLSE** for project sponsorship and guidance
- Football analytics community for inspiration

---

## 📞 Contact

**Armen** - Project Lead

Project Link: [https://github.com/yourusername/player-similarity-model](https://github.com/yourusername/player-similarity-model)

---

<p align="center">
  Made with ⚽ and ❤️ for football analytics
</p>
