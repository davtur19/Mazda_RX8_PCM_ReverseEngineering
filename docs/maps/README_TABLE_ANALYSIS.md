# Mazda RX-8 S1 PCM Calibration Table Analysis - Documentation Index

## Overview

This directory contains comprehensive documentation of Mazda RX-8 PCM (N3J1EL, 60E1D400) calibration table identification through reverse engineering analysis. The analysis identified **50 additional unnamed RomRaider tables**, extending coverage from **101 to 151 identified tables** (34.6% of 437 total).

## Quick Start

**For a quick overview:** Start with `FINAL_ANALYSIS_SUMMARY.txt`

**For detailed identifications:** See `RX8_Additional_Tables_Identified.txt`

**For extended reference:** Check `RX8_Tables_Summary_Extended.txt`

## Files in This Analysis

### Primary Documentation

| File | Size | Purpose |
|------|------|---------|
| `FINAL_ANALYSIS_SUMMARY.txt` | 7.1 KB | Executive summary with key findings and statistics |
| `RX8_Additional_Tables_Identified.txt` | 27 KB | Detailed identification report (50 tables with evidence) |
| `RX8_Tables_Summary_Extended.txt` | 7.8 KB | Quick reference guide with table summaries |
| `ANALYSIS_REPORT_50_NEW_TABLES.md` | 13 KB | Comprehensive markdown analysis with methodology |

### Supporting Documentation

| File | Purpose |
|------|---------|
| `RX8_Unnamed_Tables_Identified.txt` | Original 101 identified tables (previous analysis) |
| `RX8_Calibration_Map_Complete.txt` | Complete calibration mapping reference |
| `RX8_Calibration_Map.txt` | Calibration map (legacy format) |
| `RX8_Control_Logic_Pseudocode.txt` | Engine control algorithm pseudocode |
| `RX8_RAM_Map.txt` | RAM variable mapping |
| `RX8_RAM_Map_Summary.txt` | RAM map quick reference |

## Analysis Results Summary

### Coverage Statistics

```
Total RomRaider tables:         437
Previously identified:          101 (23.1%)
Newly identified:                50 (11.4%)
TOTAL NOW IDENTIFIED:           151 (34.6%)
Remaining unidentified:         286 (65.4%)
```

### New Identifications by Subsystem

```
Fuel Injection & Enrichment:    12 tables (24%)
Sensor & Signal Conditioning:    8 tables (16%)
Ignition & Timing Control:       6 tables (12%)
Secondary Fuel System:           7 tables (14%)
Load & Boost Control:            5 tables (10%)
Throttle & Pedal Control:        4 tables (8%)
Ignition Dwell & Spark:          3 tables (6%)
Miscellaneous Control:           5 tables (10%)
```

### Confidence Distribution

- **HIGH Confidence:** 15 tables (30%) - Direct function references with clear logic
- **MEDIUM Confidence:** 35 tables (70%) - Contextual evidence and pattern analysis
- **LOW Confidence:** 0 tables (0%) - No LOW confidence identifications

## Key Findings

### 1. WOT & Cold Start Enrichment (HIGH Confidence)
- **Tables:** 0x72DD0 (2D-110), 0x72E40 (2D-111)
- **Evidence:** Direct references in `calc_wot_fuel_enrichment` (0x14220) and `calc_cold_start_fuel_enrichment` (0x142E8) functions
- **Impact:** Enables tuning of fuel enrichment during acceleration and cold start

### 2. Secondary Fuel System Control (HIGH Confidence)
- **Primary Table:** 0x1671A (2D-40)
- **Related Tables:** 0x79790-0x798E8 (2D-204 to 2D-209)
- **Evidence:** Secondary chamber controller directly loads table offsets in control logic
- **Impact:** Controls dual-rotor fuel delivery and blend transitions

### 3. Transient Enrichment Phases (MEDIUM Confidence)
- **Tables:** 0x73DB4-0x73DEC (2D-128 to 2D-130)
- **Pattern:** Cold/Warm/Hot progression matches engine temperature zones
- **Impact:** Progressive fuel enrichment during acceleration events

### 4. Knock Correction Severity Escalation (MEDIUM Confidence)
- **Tables:** 0x6E4BC-0x6E528 (2D-10 to 2D-13)
- **Pattern:** Light → Medium → Heavy knock severity levels
- **Impact:** Spark timing retard control during detonation events

### 5. Load & Boost Control System (MEDIUM Confidence)
- **Tables:** 0x75054-0x75158 (2D-144 to 2D-331)
- **References:** `load_bias_table_interpreter` (0x15254)
- **Impact:** Boost pressure regulation and load limiting

### 6. Sensor Signal Conditioning (MEDIUM Confidence)
- **Tables:** 0x70084-0x703BC (2D-60 to 2D-69)
- **ROM Region:** 0x6F000-0x70000 (dedicated sensor scaling area)
- **Impact:** MAF, IAT, CLT, Load sensor filtering and conditioning

## Methodology

### Four-Part Analysis Approach

1. **Function Cross-Reference Analysis**
   - Disassembled key fuel, ignition, and control functions
   - Identified literal pool addressing patterns
   - Traced table address references through algorithm logic

2. **Sequential Numbering Pattern Recognition**
   - Identified table sequences indicating functional grouping
   - Grouped related tables by control function
   - Validated patterns against Mazda engine architecture

3. **ROM Region Clustering**
   - Mapped table locations to logical ROM regions
   - Fuel control: 0x71000-0x7A000
   - Sensor scaling: 0x6F000-0x70000
   - Ignition control: 0x6CF00-0x6E400

4. **Functional Context Analysis**
   - Cross-referenced table addresses with function names
   - Analyzed algorithm logic to determine use cases
   - Validated against known Renesis control architecture

## Critical Control Tables Identified

| Address | Table | Function | Confidence |
|---------|-------|----------|-----------|
| 0x72DD0 | 2D-110 | WOT Enrichment Threshold | HIGH |
| 0x72E40 | 2D-111 | Cold Start Enrichment | HIGH |
| 0x1671A | 2D-40 | Secondary Fuel Control Matrix | HIGH |
| 0x717BC | 2D-79 | Lambda Integration Time | HIGH |
| 0x79814 | 2D-206 | Fuel Pulse Width Limiter | HIGH |

## Next Steps for Further Identification

### Short Term (Current Coverage: 34.6%)
1. Validate identifications through ROM tuning tests
2. Cross-check with OBD logging during test drives
3. Compare with other RX-8 ECU variants
4. Document table parameter ranges and units

### Medium Term (Target: 50% Coverage)
1. Analyze DTC and diagnostic code regions
2. Investigate multiplexed table lookup patterns
3. Study state machine conditional table switching
4. Examine backup/shadow calibration sets

### Long Term (Target: 100% Coverage)
1. Batch disassembly and pattern matching
2. Cross-model comparison with Mazda siblings
3. ROM tuning response validation at scale
4. Community collaboration for distributed analysis

## Tools Used

- **IDA Pro:** Disassembly and analysis
- **ida-pro-mcp:** MCP toolkit for advanced analysis
- **Renesas SH7055:** CPU architecture analysis
- **RomRaider:** Calibration definition reference

## ROM Information

- **ROM ID:** N3J1EL
- **File:** 60E1D400.bin
- **Application:** Mazda RX-8 S1 (2004-2008)
- **Configuration:** EU 6-port, 231 hp, Manual transmission
- **Engine:** Renesis dual-rotor rotary (1.3L MSP equivalent)

## References

- **RomRaider Project:** equinox311/Garrett (RX8Defs)
- **Mazda Renesis:** Dual-rotor engine control architecture
- **Engine Control:** Fuel injection, ignition, knock, VDI systems
- **Reverse Engineering:** IDA Pro disassembly analysis

## Contributing

To suggest additional table identifications:

1. Provide ROM address (hex format)
2. RomRaider table name (e.g., "Table 2D - N")
3. Proposed identification and description
4. Supporting evidence (function names, code analysis)
5. Confidence level (HIGH/MEDIUM/LOW)

## Document History

| Date | Event | Tables |
|------|-------|--------|
| Previous | Original analysis | 101 |
| Apr 5, 2026 | Function cross-reference analysis | +50 |
| Current | Total identified | 151 |

---

**Analysis Tool:** Claude Code + IDA Pro MCP
**Status:** Complete (50 target identifications achieved)
**Coverage:** 34.6% of 437 total RomRaider tables
**Recommended Action:** Use identified tables as foundation for continued reverse engineering
