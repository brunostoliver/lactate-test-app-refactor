# Lactate Test App Refactor

SwiftUI iOS app for recording, analyzing, comparing, and exporting lactate threshold tests for multiple athletes.

## Current Features

- Athlete-first workflow with SwiftData-backed athlete and test storage
- Create and manage multiple athletes
- Store athlete profile details including date of birth and gender
- Create, view, edit, and compare lactate tests
- Filter saved tests by sport and date range
- Graph-based threshold analysis and 5-zone summaries
- Export saved tests as JSON, CSV, and PDF
- App-wide appearance and unit preferences
- Adaptive iPad layouts for landscape and portrait athlete workflows

## Data Model

The app uses SwiftData for persistence.

Core entities:

- `Athlete`
- `LactateTest`
- `LactateStep`

Athletes can also store profile metadata such as:

- date of birth
- gender

Tests can also store environment metadata such as:

- temperature
- humidity
- terrain / place

## Project Structure

- [AdaptiveRootView.swift](./AdaptiveRootView.swift): adaptive iPad/phone root routing, including wide iPad pane layouts and portrait athlete workspace behavior.
- [AthleteListView.swift](./AthleteListView.swift): first screen of the app; lets the user create a new athlete, choose an existing athlete, and adjust global appearance and unit preferences.
- [AthleteProfileEditorView.swift](./AthleteProfileEditorView.swift): reusable athlete profile form for creating and editing athlete name, date of birth, and gender.
- [ContentView.swift](./ContentView.swift): main screen coordinator; drives the athlete detail view, test editor flow, compare behavior, filtering state, sheets, alerts, and screen-level navigation logic.
- [ContentView+Sections.swift](./ContentView+Sections.swift): contains most SwiftUI section views used by `ContentView`, including saved tests, forms, graph area, threshold summaries, filters, and action buttons.
- [ContentView+Analysis.swift](./ContentView+Analysis.swift): contains threshold-analysis logic, graph point generation, Dmax and breakpoint calculations, and training-zone calculations.
- [ContentView+Export.swift](./ContentView+Export.swift): contains export generation for JSON, CSV, and PDF, including export summaries and export-oriented helper calculations.
- [ContentView+Formatting.swift](./ContentView+Formatting.swift): display formatting helpers for labels, dates, workloads, and values shown in the UI.
- [ContentViewSupport.swift](./ContentViewSupport.swift): shared UI support and app enums such as graph axis selection, appearance mode, search filters, export errors, and logo/header helpers.
- [LactateChartView.swift](./LactateChartView.swift): reusable chart view that renders the lactate curve, selected points, and threshold markers.
- [FullScreenLactateChartView.swift](./FullScreenLactateChartView.swift): full-screen graph presentation with zoom controls for inspecting lactate curves.
- [ExportLactateChartView.swift](./ExportLactateChartView.swift): simplified chart view used specifically for exported reports and PDF generation.
- [StepEditor.swift](./StepEditor.swift): reusable editor for a single lactate test step, including heart rate, power, pace, speed, and lactate inputs.
- [PaceInput.swift](./PaceInput.swift): focused input helper for entering and converting running pace values across unit systems.
- [SampleTestCatalog.swift](./SampleTestCatalog.swift): central catalog of built-in running and cycling sample tests used for demo loading.
- [SampleTestPickerView.swift](./SampleTestPickerView.swift): separate sample-test browser that lets the user load one sample into the editor or import all samples.
- [ShareSheet.swift](./ShareSheet.swift): UIKit bridge used to present the iOS share sheet for exported files.
- [Formatters.swift](./Formatters.swift): generic pace and speed formatting utilities plus unit-preference helpers used across the app.
- [DateFormatter+PDFTimestamp.swift](./DateFormatter+PDFTimestamp.swift): date formatter extension used for PDF/export timestamps.
- [Models.swift](./Models.swift): app-level domain models such as `Athlete`, `LactateTest`, `LactateStep`, drafts, sport enums, and test metadata.
- [PersistenceModels.swift](./PersistenceModels.swift): SwiftData `@Model` entities used to persist athletes, tests, and steps.
- [PersistenceMapping.swift](./PersistenceMapping.swift): mapping layer between app domain models and SwiftData persistence entities.
- [SwiftDataTestStore.swift](./SwiftDataTestStore.swift): observable store that loads, saves, updates, deletes, and queries athletes and tests through SwiftData.
- [MigrationService.swift](./MigrationService.swift): migration and normalization logic used to move older saved data into the current SwiftData structure.
- [Lactate_test_app_v3.swift](./Lactate_test_app_v3.swift): app entry point; configures the model container and launches the root SwiftUI view.
- [LactateStorageCleanupService.swift](./LactateStorageCleanupService.swift): legacy cleanup helper related to older storage approaches before the current SwiftData workflow.

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
- `feature/ipad-layout`: iPad-specific workspace and multi-pane layout work
- `feature/athlete-profile`: athlete DOB/gender profile fields in support of later VO2 max work
