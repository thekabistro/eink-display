# Transit Map Implementation Plan

## Technical Approach
**Language**: ESPHome YAML + C++ Lambda Functions  
**Framework**: Existing ESPHome setup with custom drawing logic  
**API**: OneBusAway REST API for real-time train data  

## Step-by-Step Implementation

---

### Step 1: Display Rotation & Basic Layout

**Goal**: Rotate display to vertical orientation and establish basic structure

**Changes**:
- Change `rotation: 0` to `rotation: 90` in `eink.yaml` 
- Replace existing weather lambda with simple "Hello Transit" text
- Update display dimensions in code (swap width/height references)

**Acceptance Criteria**:
- [ ] Display rotates to vertical orientation (480×800)
- [ ] Text appears correctly oriented 
- [ ] No display corruption or errors

**Human Tester Steps**:
1. Flash updated configuration to ESP32
2. Verify display shows "Hello Transit" text in vertical orientation
3. Check that text is readable and properly oriented
4. Confirm no visual artifacts or corruption

---

### Step 2: Static Route Line & Stations

**Goal**: Draw the transit route with all stations positioned correctly

**Changes**:
- Draw vertical line down center of display
- Calculate Y positions for all 23 Link stations
- Add station dots (●) at correct positions
- Display station names next to dots

**Acceptance Criteria**:
- [ ] Vertical line runs from top to bottom of transit area
- [ ] 23 station dots evenly spaced along the line
- [ ] All station names visible and readable
- [ ] Proper spacing between elements (no overlap)

**Human Tester Steps**:
1. Flash configuration and verify visual layout
2. Count stations (should be 23 total)
3. Check station names match actual Link stations
4. Verify spacing looks proportional and readable
5. Take photo for reference

---

### Step 3: Mock Train Data

**Goal**: Test train positioning logic with fake data

**Changes**:
- Create hardcoded mock train data in C++
- Position 2-3 trains at different locations ("T1N", "T2S", "T3N")
- Implement train symbol positioning between stations
- Test positioning calculations

**Acceptance Criteria**:
- [ ] 2-3 train symbols appear on the route line
- [ ] Trains positioned between or at stations (not floating)
- [ ] Direction indicators (N/S) display correctly
- [ ] Train IDs are readable and unique

**Human Tester Steps**:
1. Flash and verify trains appear as expected
2. Check that train positions look realistic relative to stations
3. Verify train symbols don't overlap with station names
4. Confirm direction indicators make sense (N trains going up, S trains going down)

---

### Step 4: OneBusAway API Integration

**Goal**: Connect to real-time transit data

**Changes**:
- Add HTTP request component to ESPHome
- Create API call to OneBusAway endpoint
- Parse JSON response to extract train data
- Add error handling for API failures

**Acceptance Criteria**:
- [ ] Successfully connects to OneBusAway API
- [ ] JSON response parsed without errors
- [ ] Train data extracted (ID, direction, position, next stop)
- [ ] Graceful handling when API is unavailable
- [ ] Console logging shows parsed train data

**Human Tester Steps**:
1. Provide OneBusAway API key for configuration
2. Flash and monitor ESP32 console logs
3. Verify API calls succeed (check HTTP response codes)
4. Confirm train data appears in logs with realistic values
5. Test behavior when WiFi disconnected (should show error/fallback)

---

### Step 5: Dynamic Train Updates

**Goal**: Display live trains moving on the route

**Changes**:
- Calculate real train positions between stations using timing data
- Update display every 2-3 minutes with fresh data
- Handle edge cases (no active trains, invalid data)
- Optimize for e-ink refresh patterns

**Acceptance Criteria**:
- [ ] Trains appear at realistic positions between stations
- [ ] Train positions update over time as trains move
- [ ] Display refreshes every 2-3 minutes automatically
- [ ] Handles scenarios with 0, 1, or 6+ trains gracefully
- [ ] No display corruption during updates

**Human Tester Steps**:
1. Flash and observe for 10-15 minutes
2. **Real-world validation**: Check if displayed train positions match actual train locations (use OneBusAway mobile app for comparison)
3. Verify trains move in the correct direction over time
4. Test during different times of day (rush hour vs off-peak)
5. Monitor for any display refresh issues or corruption
6. Confirm automatic updates occur every 2-3 minutes

---

## Testing Strategy

### Incremental Validation
Each step builds on the previous one, with immediate visual feedback to catch issues early.

### Real-World Testing
Step 5 requires comparison with actual Link train positions using:
- OneBusAway mobile app
- Sound Transit real-time arrivals
- Physical observation if near a station

### Error Handling Testing
- WiFi disconnection during operation
- API rate limiting or outages  
- Invalid/malformed API responses
- Power cycling during updates

## Human-in-Loop Collaboration

### What I'll Provide
- Complete YAML configuration files for each step
- Clear explanation of changes before implementation
- Debugging assistance when issues arise
- Code comments explaining complex positioning logic

### What You'll Provide
- **Physical testing**: Flash each step to your actual ESP32/display
- **OneBusAway API key**: Required for Step 4+
- **Real-world validation**: Confirm trains appear where they actually are
- **UX feedback**: Font sizes, spacing, layout improvements
- **Edge case testing**: Different times of day, network conditions

### Communication Protocol
- I'll explain each change before making it
- You test each step completely before we proceed to the next
- We iterate on visual design together based on your feedback
- You have final say on spacing, fonts, and visual elements

## Success Criteria

**Final deliverable**: A working e-ink display that shows:
- All 23 Link stations in correct north-south order
- Real-time train positions updated every 2-3 minutes
- Clear train identification and direction indicators
- Robust error handling for network/API issues
- Optimized refresh patterns for e-ink longevity