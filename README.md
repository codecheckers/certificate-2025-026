# CODECHECK certificate 2025-026

Repository for CODECHECK certificate 2025-026.<br>
Report: https://zenodo.org/records/{placeholder_identifier}

Publication: [Automated validation of route instructions in indoor environments](https://doi.org/10.5311/JOSIS.2025.30.385)<br>
Code Repository: https://doi.org/10.6084/m9.figshare.24208518

If you find the paper or this repository helpful in your publications, please consider citing it.

```bibtex
@article{Arabsheibani2025,
  author  = {Arabsheibani, Reza and Winter, Stephan and Tomko, Martin},
  journal = {Journal of Spatial Information Science (JOSIS)},
  title   = {Automated validation of route instructions in indoor environments},
  year    = {2025},
  number  = {30},
  pages   = {25–60},
  doi     = {https://doi.org/10.5311/JOSIS.2025.30.385}
}
```

If you want to use the checked code in your publications, please consider citing it.

```bibtex
@article{Arabsheibani2023,
  author = "Reza Arabsheibani",
  title  = "{Unlocking Navigation Success: Using Instruction-following Agents to Explore the Role of Floorplan Complexity in Route Instructions Validity.}",
  year   = "2023",
  month  = "9",
  url    = "https://figshare.com/articles/software/Unlocking_Navigation_Success_Using_Instruction-following_Agents_to_Explore_the_Role_of_Floorplan_Complexity_in_Route_Instructions_Validity_/24208518",
  doi    = "10.6084/m9.figshare.24208518.v1"
}
```

If you want to reproduce the code yourself again, then first run the following command to build the Docker image:

```bash
docker build -f CodecheckDockerfile -t codecheck-2025-026 .
```

Next run the following command to start the docker container, from where all scripts can be executed.

```bash
docker run --rm -it -v "$PWD:/codecheck" -w /codecheck codecheck-2025-026
```

- - -

# Route Instruction Generation and validation for TextWorld Environments

This repository contains a comprehensive pipeline for generating synthetic indoor environments from font glyphs, converting them to TextWorld game formats, calculating complexity metrics, generating route instructions, and validating navigation through automated agents.

## Overview

The pipeline consists of five main stages:

1. **Generating Synthetic Environments** - Creating geometric environments from font glyphs
2. **Converting to TextWorld Format** - Transforming GeoJSON environments into playable TextWorld games
3. **Calculating Complexity Criteria** - Computing spatial complexity metrics
4. **Finding Longest Shortest Paths** - Identifying optimal routes for navigation
5. **Navigation Validation** - Testing route instructions with automated agents

## Repository Structure

```
RouteInstructionGeneration/
├── Fonts/                          # Font files used for environment generation
├── geojson/                        # Generated GeoJSON environments
│   └── Curated7/                   # Curated dataset (shapefiles)
├── games/                          # Generated TextWorld game files (.z8, .ni)
├── data/                           # Intermediate data files
│   ├── RI/                         # Route instruction XML files
│   └── [letter]_*.geojson          # Per-letter environment components
├── dicts.py                        # Configuration dictionaries (letters, landmarks)
├── direction.py                    # Direction mapping utilities
├── concepts.py                     # TextWorld concept definitions
├── templates.py                     # Inform7 code templates
├── generator.py                    # Core game generator (TextWorld_WP1 compatible)
├── creating_ni_2.py                # Game production from GeoJSON
├── RouteFromPath.py                # Route instruction generation utilities
├── Complexity_Criteria_Finder_parallel.py  # Complexity metric calculation
├── Generate_Route_Instructions_LongestShortestV4.py  # LSP route generation
├── Navigate_Parallel.py            # Parallel navigation validation
├── Run_Experiment.py               # Experiment runner
└── [other utility scripts]
```

## Dependencies

### Core Libraries
- **geopandas** - Geospatial data manipulation
- **shapely** - Geometric operations
- **networkx** - Graph algorithms
- **momepy** - Urban morphology analysis
- **textworld** - Text-based game framework
- **freetype-py** - Font glyph extraction
- **osmnx** - Spatial network analysis
- **pandas** - Data manipulation
- **openpyxl** - Excel file handling
- **xml.etree.ElementTree** - XML parsing

### Installation

```bash
pip install geopandas shapely networkx momepy textworld freetype-py osmnx pandas openpyxl
```

**Note**: TextWorld requires **Inform7** compiler to be installed on your system. Install Inform7 from [https://ganelson.github.io/inform/](https://ganelson.github.io/inform/) and ensure it's accessible in your system PATH.

## Quick Start

To run the complete pipeline:

1. **Generate environments** (if not already done):
   ```python
   from Run_Experiment import create_Polygons_from_Letters
   create_Polygons_from_Letters(folder_name='Curated7', version='V7')
   ```

2. **Create room/door/landmark files** (if not already done):
   ```python
   python create_rooms_parallel.py
   ```

3. **Generate games** (for each letter/route combination):
   ```python
   from creating_ni_2 import produce_game
   produce_game(letterz='A_perspire_Approach1', x_origin='...', y_origin='...', primary_orientation=6)
   ```

4. **Calculate complexity metrics**:
   ```python
   # Edit Complexity_Criteria_Finder_parallel.py to set condition=True
   python Complexity_Criteria_Finder_parallel.py
   ```

5. **Generate route instructions**:
   ```python
   python Generate_Route_Instructions_LongestShortestV3.py
   ```

6. **Validate navigation**:
   ```python
   # Edit Navigate_Parallel.py to set correct XML file path
   python Navigate_Parallel.py
   ```

## Pipeline Workflow

### Stage 1: Generating Synthetic Environments

**Purpose**: Create geometric environments from font glyphs with landmarks and spatial structure.

**Key Files**:
- `dicts.py` - Defines letters to process and landmark types
- `direction.py` - Direction mappings (cardinal, relative)
- `concepts.py` - TextWorld concept classes (Player, IndoorArea, IndoorRoom, Door, Landmark)
- `templates.py` - Inform7 code templates for game generation
- `Run_Experiment.py` - Main script for environment generation

**Process**:
1. Load font files from `Fonts/` directory
2. Extract glyph outlines for letters defined in `dicts.letters`
3. Convert glyphs to polygons (GeoJSON format)
4. Generate random landmarks within each letter polygon
5. Create skeletonized network structure
6. Save environments as shapefiles in `geojson/Curated7/`

**Output**: Shapefiles containing:
- `ICD_CalculatedV7.shp` - Letter polygons with distinct identifiers
- `OneLetterV7.shp` - Individual letter geometries
- `SkelV7.shp` - Skeletonized network structure
- `landmarksV7.shp` - Landmark points

**Usage**:
```python
from Run_Experiment import create_Polygons_from_Letters
create_Polygons_from_Letters(folder_name='Curated7', version='V7')
```

### Stage 2: Converting to TextWorld Format

**Purpose**: Transform GeoJSON environments into playable TextWorld games (.z8 format).

**Key Files**:
- `creating_ni_2.py` - Main conversion script (`produce_game()` function)
- `RouteFromPath.py` - Path finding utilities
- `create_rooms_parallel.py` - Room decomposition (parallel processing)

**Process**:
1. Load shapefiles for a specific letter
2. Decompose letter polygon into rooms (using cross-line splitting)
3. Extract skeleton network and convert to graph
4. Identify decision points (nodes with degree ≥ 3)
5. Create indoor areas from graph nodes
6. Identify doors at room intersections
7. Assign landmarks to areas
8. Generate Inform7 code using templates
9. Compile to TextWorld game format (.z8)

**Output**: 
- `games/random/[letter]_[x_origin]_[y_origin].z8` - Playable game files
- `data/[letter]_room.geojson` - Room geometries
- `data/[letter]_door.geojson` - Door locations
- `data/[letter]_landmark.geojson` - Landmark locations
- `data/[letter]_skeleton.geojson` - Skeleton network
- `data/[letter]_graphNodes.geojson` - Graph node locations
- `data/[letter]_graphEdges.geojson` - Graph edge connections

**Usage**:
```python
from creating_ni_2 import produce_game
produce_game(letterz='A_perspire_Approach1', 
            x_origin='500748.67', 
            y_origin='1028060.38', 
            primary_orientation=6)
```

### Stage 3: Calculating Complexity Criteria

**Purpose**: Compute spatial complexity metrics for each environment.

**Key File**: `Complexity_Criteria_Finder_parallel.py`

**Metrics Calculated**:
1. **ICD (Intersection Count Density)** - `2 * edges / nodes`
2. **Bearing Entropy** - Shannon entropy of edge bearings
3. **Categorized Bearing Entropy** - Entropy of bearings in 10 categories (36° each)
4. **Graph Asymmetry** - Rotation angles required for node symmetry

**Process**:
1. Load letter polygons and skeleton networks
2. Convert skeleton to NetworkX graph
3. Calculate bearings for all edges
4. Compute entropy metrics
5. Calculate graph asymmetry (for nodes with degree 3-4)
6. Normalize all metrics to [0,1] range
7. Categorize normalized values into deciles

**Output**: Excel file (`Complexity_Criteria_for_v*.xlsx`) with columns:
- Letter, ICD, Bearing Entropy, Categorized Bearing Entropy, Graph Asymmetry
- Normalized versions of all metrics
- Skeletonization_Method, Path

**Usage**:
```python
# Edit the main block in Complexity_Criteria_Finder_parallel.py
# Set the condition to True and run:
python Complexity_Criteria_Finder_parallel.py
```

### Stage 4: Finding Longest Shortest Paths and Generating Route Instructions

**Purpose**: Identify the longest shortest path in each environment and generate route instructions in multiple styles and grammars.

**Key Files**: 
- `Generate_Route_Instructions_LongestShortestV4.py` - Finds LSP paths only
- `Generate_Route_Instructions_LongestShortestV3.py` - Finds LSP paths AND generates route instructions

**Process**:
1. Load letter polygons and skeleton networks
2. Convert skeleton to NetworkX graph
3. Calculate all-pairs shortest path lengths using Dijkstra's algorithm
4. Find the pair with maximum shortest path length
5. Extract the actual path between origin and destination
6. Generate route instructions using `RouteFromPath.py`:
   - **Styles**: Turn-based, Landmark-based, Hybrid
   - **Grammars**: 4sector, 6sector, 8sector, Klippel
   - Instructions use relative directions (turn left, go straight, etc.)
   - Landmark references when landmarks are near path edges
7. Save instructions to XML format

**Route Instruction Generation** (`RouteFromPath.py`):
- Converts path edges to relative turn instructions
- Incorporates landmarks when within proximity threshold
- Supports multiple grammar systems (4/6/8 sector, Klippel)
- Generates step-by-step navigation instructions

**Output**: 
- XML file (`data/RI/Route_Instructions_LongestShortestV*.xml`) with structure:
  ```xml
  <letter name="...">
    <route x_origin="..." y_origin="..." x_destination="..." y_destination="...">
      <style name="Turn-based|Landmark|Hybrid">
        <grammar name="4sector|6sector|8sector|Klippel">
          <route_instruction>Go straight. Turn left. ... Arrive at destination.</route_instruction>
        </grammar>
      </style>
    </route>
  </letter>
  ```
- CSV file (`data/RI/Route_Instructions_LongestShortest_length.csv`) with path lengths

**Usage**:
```python
# For path finding only:
python Generate_Route_Instructions_LongestShortestV4.py

# For path finding + instruction generation:
python Generate_Route_Instructions_LongestShortestV3.py
```

### Stage 5: Navigation Validation

**Purpose**: Validate route instructions by navigating agents through TextWorld games.

**Key Files**:
- `Navigate_Parallel.py` - Parallel navigation testing
- `Read_Route_Instructions.py` - XML route instruction reader

**Process**:
1. Load route instructions from XML file
2. For each letter, route, style, and grammar combination:
   - Load or generate the corresponding game file
   - Initialize TextWorld environment
   - Execute route instructions step-by-step
   - Extract final coordinates from game state
   - Compare with intended destination
   - Mark as Valid/Invalid
3. Save results to Excel file

**Output**: Excel file with columns:
- Letter, Origin_X, Origin_Y, Destination_X, Destination_Y
- Grammar, Valid/Invalid

**Usage**:
```python
# Edit the XML file path in Navigate_Parallel.py
python Navigate_Parallel.py
```

## Configuration

### Letter Selection

Edit `dicts.py` to modify:
- `letters` - List of letter combinations to process
- `landmarks_dict` - Available landmark types
- `problematic` - Letters to exclude

### Font Configuration

Place font files (.ttf) in `Fonts/` directory. The system supports multiple fonts:
- perspire.ttf (default)
- Average-Regular.ttf
- Arial variants
- And others

### Coordinate System

All spatial data uses **EPSG:32639** (WGS 84 / UTM zone 39N). Conversion to lat/long (EPSG:4326) is handled automatically for bearing calculations.

## Data Flow

```
Font Files → GeoJSON Environments → TextWorld Games
                ↓
         Complexity Metrics
                ↓
         Route Instructions (XML)
                ↓
         Navigation Validation (Excel)
```

## Data Directory Structure

The `data/` directory contains intermediate files organized by letter:

```
data/
├── RI/                                    # Route Instructions
│   ├── Route_Instructions_LongestShortestV*.xml
│   └── Route_Instructions_LongestShortest_length.csv
└── [letter]_*.geojson                     # Per-letter files
    ├── [letter]_room.geojson              # Room polygons
    ├── [letter]_door.geojson              # Door points
    ├── [letter]_landmark.geojson          # Landmark points
    ├── [letter]_skeleton.geojson          # Skeleton network
    ├── [letter]_graphNodes.geojson        # Graph nodes
    └── [letter]_graphEdges.geojson       # Graph edges
```

## Route Instruction Styles and Grammars

### Styles
- **Turn-based**: Uses only directional instructions (turn left, go straight)
- **Landmark**: Uses landmark references (pass the desk, go toward chair)
- **Hybrid**: Combines both turn-based and landmark instructions

### Grammars
- **4sector**: 4-directional (straight, left, right, around)
- **6sector**: 6-directional (adds slight left/right)
- **8sector**: 8-directional (adds sharp left/right)
- **Klippel**: Based on Klippel et al. spatial relations model

## Important Notes

1. **Parallel Processing**: Several scripts use multiprocessing for efficiency:
   - `Complexity_Criteria_Finder_parallel.py`
   - `Navigate_Parallel.py`
   - `create_rooms_parallel.py`

2. **File Naming Convention**: 
   - Letters are identified as `[letter]_[font]_[approach]`
   - Games are named `[letter]_[x_origin]_[y_origin].z8`

3. **TextWorld Integration**: The codebase extends TextWorld with:
   - 8-directional navigation (N, S, E, W, NE, NW, SE, SW)
   - Relative directions (front, back, left, right, sharp left/right, slight left/right)
   - Ego-centric frame of reference
   - Indoor space concepts (rooms, areas, doors, landmarks)

4. **Excluded Folder**: The `TextWorld_WP1/` folder contains earlier work and is excluded from this pipeline.

## Utility Scripts

Additional scripts for specific tasks:

- **`dissolv.py`** - Alternative environment generation script
- **`main.py`** - Simple font glyph extraction example
- **`main_merge.py`** - Font glyph extraction with character set
- **`countRooms.py`** - Room counting utilities
- **`LettersConnected.py`** - Letter connectivity analysis
- **`Complexity_Criteria_Finder_parallel.py`** - Also includes functions for:
  - Joining Excel files (`join_excel()`)
  - Adding agent validation results (`add_agent_grammar_to_excel()`)

## Troubleshooting

### Common Issues

1. **Missing Font Files**: Ensure all required fonts are in `Fonts/` directory
2. **Shapefile Errors**: Verify GeoJSON/Shapefile CRS is EPSG:32639
3. **TextWorld Compilation Errors**: Check Inform7 installation and game file paths
4. **Memory Issues**: Reduce parallel processing cores or process letters in batches

### Dependencies Conflicts

If you encounter issues with `momepy` or `osmnx`, ensure compatible versions:
```bash
pip install momepy==0.6.0 osmnx==1.6.0
```

## Citation

If you use this codebase, please cite the associated research paper.
Arabsheibani, Reza, Stephan Winter, and Martin Tomko. "Automated validation of route instructions in indoor environments." Journal of Spatial Information Science 30 (2025): 25-60.

## Contact

rasheibani@gmail.com

