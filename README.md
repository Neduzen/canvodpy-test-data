# canvod-readers Test Data

Test data for canvod-readers package validation.

## Structure

```
test_data/
├── 01_Rosalia/                          # Rosalia site test files
│   ├── 01_reference/01_GNSS/01_raw/
│   │   └── 25001/                       # 2025 DOY 001
│   └── 02_canopy/01_GNSS/01_raw/
│       └── 25001/
│           └── ract001a00.25o          # ✅ Single 15-min file for tests
├── valid/                               # Alternative structure (future)
│   ├── rinex_v3_04/
│   └── aux/
└── README.md                            # This file
```

## Current Test Files

### Rosalia Canopy - 2025 DOY 001

**File**: `01_Rosalia/02_canopy/01_GNSS/01_raw/25001/ract001a00.25o`

- **Size**: ~1.5 MB
- **Format**: RINEX v3 observation (short name format)
- **Period**: 2025-01-01 00:00-00:15 UTC
- **Receiver**: Below-canopy receiver
- **Site**: Rosalia, Austria
- **Purpose**: Integration tests for RINEX parsing

## Usage in Tests

Tests use the `sample_rinex_file` fixture from `conftest.py`:

```python
def test_something(sample_rinex_file):
    from canvod.readers import Rnxv3Obs
    
    obs = Rnxv3Obs(fpath=sample_rinex_file)
    ds = obs.to_ds()
    # ... test assertions
```

## Adding More Test Files

### For Error Handling Tests

Create **corrupted** directory with intentionally broken files:

```
test_data/
└── corrupted/
    ├── truncated_header.25o      # Missing END OF HEADER
    ├── invalid_epochs.25o        # Non-monotonic times
    └── bad_satellites.25o        # Invalid SV IDs
```

### For Edge Case Tests

Create **edge_cases** directory:

```
test_data/
└── edge_cases/
    ├── minimal.25o               # Single epoch
    ├── sparse.25o                # Large time gaps
    └── multi_gnss.25o            # All GNSS systems
```

## File Naming Convention

RINEX short name format:

```
ract001a00.25o
└┬┘└┬┘└┬┘└┬┘└┬─┬┘
 │  │  │  │  │ └─ Extension: o (observation)
 │  │  │  │  └─── Year: 25 (2025)
 │  │  │  └────── Session: 00 (00:00-00:15)
 │  │  └───────── Hour: a (00h)
 │  └──────────── DOY: 001
 └─────────────── Station: ract (Rosalia canopy)
```

## Source

Test files are copied from the main [canvodpy-demo](https://github.com/nfb2021/canvodpy-demo) repository.

## Maintenance

**Keep test files minimal**:
- Use single-epoch or single-file when possible
- Don't duplicate full datasets
- Document the purpose of each test file

**When updating**:
1. Add new test file to appropriate directory
2. Update this README
3. Create corresponding test case
4. Commit to canvodpy-test-data repository
5. Update main repo submodule reference

## License

MIT (same as canvodpy)

Data collected by TU Wien Department of Geodesy and Geoinformation.
