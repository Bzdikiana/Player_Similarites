# ═══════════════════════════════════════════════════════════════════════════════
#                         CHANGELOG
#                  Player Similarity Model
# ═══════════════════════════════════════════════════════════════════════════════

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Planned
- Learned feature weights from expert labels
- Graph Attention Networks (GAT) for team structure
- Opposition-adjusted metrics
- Multi-season trend analysis
- Video-based feature extraction

---

## [1.0.0] - 2026-02-21

### Added

#### Core Features
- **SimilarityConfig**: Centralized configuration management
- **PitchNormalizer**: Coordinate normalization for consistent attacking direction
- **ExpectedThreatModel**: xT computation for pitch zones

#### Feature Extraction (Part 2)
- **BehavioralFeatureExtractor**: 7 feature blocks
  - Spatial patterns (96 dimensions)
  - Passing style (~50 dimensions)
  - Receiving patterns (~30 dimensions)
  - Pressing behavior (~30 dimensions)
  - Transition play (~20 dimensions)
  - Value added (~20 dimensions)
  - Action chains (~165 dimensions)
- **PlayerEmbeddingPipeline**: Orchestrates extraction for all players
- **RoleDiscovery**: GMM-based tactical role clustering
- **RatioBasedSimilarity**: Ratio-based similarity with sigmoid weighting

#### Context-Aware Analysis (Part 3)
- **ScenarioEmbedder**: Score state, game phase, competition encoding
- **SpatialEmbedder**: 360 freeze-frame spatial features
- **TeamGraphBuilder**: Pass network construction
- **SimpleGNNLayer**: 2-layer Graph Convolutional Network
- **ContextAwarePlayerEmbedding**: Unified context-aware embeddings
- **ContextAwareSimilarityAPI**: Context-specific queries

#### Explainability (Part 4)
- **ExplainableSimilarityAnalyzer**: Multi-dimensional similarity breakdown
- **FeatureContributionAnalyzer**: Per-feature drill-down
- **UnifiedExplainableSimilarityAPI**: User-friendly interface

#### Visualization
- Radar charts for style profiles
- Context heatmaps
- Comprehensive dashboards
- Feature contribution plots

### Technical Details
- 347 players processed from 30 sample matches
- ~411 raw features → 32 PCA dimensions
- 3 discovered roles (inverted_fullback, deep_playmaker_6, box_to_box_8)

### Known Limitations
- Feature weights manually tuned (not learned)
- Limited to StatsBomb open data competitions
- Basic GNN architecture (2-layer GCN)
- No opposition strength adjustment

---

## [0.1.0] - 2026-02-20

### Added
- Initial project structure
- Basic data loading from StatsBomb API
- Exploratory data analysis notebook

---

## Future Roadmap

### v1.1.0 (Planned)
- Improved feature weights
- Better action chain vocabulary
- More granular passing features

### v1.2.0 (Planned)
- Attention-based context encoding
- Multi-competition support
- Performance optimizations

### v2.0.0 (Planned)
- Learned embeddings (neural network approach)
- Tracking data integration
- Real-time updates

---

## Migration Notes

### From v0.x to v1.0.0
- Complete rewrite of feature extraction
- New API surface (UnifiedExplainableSimilarityAPI)
- Breaking changes in all class interfaces

---

[Unreleased]: https://github.com/yourusername/player-similarity-model/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/yourusername/player-similarity-model/releases/tag/v1.0.0
[0.1.0]: https://github.com/yourusername/player-similarity-model/releases/tag/v0.1.0
