# Lactate Test App Refactor

SwiftUI iOS app for recording, analyzing, comparing, and exporting lactate threshold tests for multiple athletes.

## Current Features

- Athlete-first workflow with SwiftData-backed athlete and test storage
- Create and manage multiple athletes
- Create, view, edit, and compare lactate tests
- Filter saved tests by sport and date range
- Graph-based threshold analysis and 5-zone summaries
- Export saved tests as JSON, CSV, and PDF
- App-wide appearance and unit preferences

## Data Model

The app uses SwiftData for persistence.

Core entities:

- `Athlete`
- `LactateTest`
- `LactateStep`

Tests can also store environment metadata such as:

- temperature
- humidity
- terrain / place

## Project Structure

- [AthleteListView.swift](./AthleteListView.swift): athlete landing screen
- [ContentView.swift](./ContentView.swift): athlete detail and editor flow orchestration
- [ContentView+Sections.swift](./ContentView+Sections.swift): UI sections
- [ContentView+Analysis.swift](./ContentView+Analysis.swift): threshold and graph analysis
- [ContentView+Export.swift](./ContentView+Export.swift): export logic
- [ContentView+Formatting.swift](./ContentView+Formatting.swift): formatting helpers
- [ContentViewSupport.swift](./ContentViewSupport.swift): shared enums, models, UI support
- [SwiftDataTestStore.swift](./SwiftDataTestStore.swift): SwiftData-backed store
- [PersistenceModels.swift](./PersistenceModels.swift): SwiftData entities
- [PersistenceMapping.swift](./PersistenceMapping.swift): mapping between app models and persistence entities

## Workflow Notes

This repo is used as the Windows-side refactor workspace, while the app is validated in Xcode on macOS.

If Swift files are copied into the Xcode project manually, clean non-breaking spaces afterward:

```bash
find "/Users/rentamac/Documents/Lactate-test-app_v3/Lactate test app_v3" -name "*.swift" -print0 | xargs -0 perl -pi -e 's/\xC2\xA0/ /g'
```

Then in Xcode:

1. Clean Build Folder
2. Build again

## Git Branches

Recent work has been organized around:

- `main`: stable merged baseline
- `feature/athlete-tracker`: athlete-first SwiftData workflow
- `feature/athlete-search`: athlete-level filtering, comparison, and UI refinements

