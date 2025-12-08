# E-ink Transit Map Implementation Plan

## High-Level Concept

Transform the existing ESP32 e-ink weather dashboard to display real-time Seattle Link Light Rail train positions. The display will show a vertical route map with live train locations updated via the OneBusAway API.

## Visual Design

### Screen Orientation: Vertical (480×800)
The display will be rotated 90 degrees from current weather-only layout to better accommodate the north-south oriented Link rail system.

### Layout Structure
```
┌─────────────────────┐
│ Header (80px)       │  ← Date/Time
├─────────────────────┤
│                     │
│                     │
│                     │
│ Transit Map         │  ← Main transit visualization
│ (720px)             │    Full Link Line 1 route
│                     │    Real-time train positions  
│                     │
│                     │
│                     │
└─────────────────────┘
```

## Transit Map Visualization

### Route Representation
- **Continuous line**: Single vertical line representing the Link Light Rail track
- **Station dots**: Circles (●) positioned along the line for each of the 23 stations
- **Station names**: Left-aligned text next to each station dot
- **Train indicators**: Text symbols ("T1", "T2", etc.) positioned along the line based on real-time location

### Station Layout (North to South)
```
Lynnwood City Center    ●
Mountlake Terrace       ●
Shoreline North/185th   ●
Shoreline South/148th   ●
Northgate               ●
Roosevelt               ●
U District              ●
University of Washington●
Capitol Hill            ●
Westlake               ●
Symphony                ●
Pioneer Square          ●
Int'l Dist/Chinatown   ●
Stadium                 ●
SODO                   ●
Beacon Hill            ●
Mount Baker            ●
Columbia City          ●
Othello                ●
Rainier Beach          ●
Tukwila Int'l Blvd     ●
SeaTac/Airport         ●
Angle Lake             ●
```

### Train Position Logic
- Parse OneBusAway API data to get train positions relative to stations
- Calculate position along the line based on:
  - Next station
  - Time to next station
  - Total leg time
- Display trains as "T1", "T2", etc. positioned accurately along the route

## Key Design Decisions

### Why "T1", "T2" Text Symbols?
- **Monochrome compatibility**: Works perfectly with e-ink limitations
- **Clear identification**: Easy to distinguish multiple trains
- **Scalable**: Can handle 6+ trains without visual clutter
- **Information rich**: Can add direction suffix (T1N, T2S) if needed

### Why Vertical Orientation?
- **Geographic accuracy**: Matches actual north-south route alignment
- **Full route display**: All 23 stations fit comfortably with proper spacing
- **Natural scrolling**: Future expansion (Line 2) follows logical vertical flow
- **Maximizes screen real estate**: Full focus on transit information

### Why Continuous Line with Dots?
- **Visual clarity**: Easy to see the complete route at a glance
- **Position accuracy**: Trains can be placed anywhere along the line, not just at stations
- **Familiar representation**: Matches standard transit map conventions
- **Scalable**: Easy to add additional lines in the future