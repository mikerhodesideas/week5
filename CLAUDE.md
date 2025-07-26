# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

- `npm run dev` - Start Next.js development server (user will handle this)
- `npm run build` - Build the application for production
- `npm run start` - Start production server
- `npm run lint` - Run ESLint to check code quality

## Project Architecture

This is a Next.js 15 Google Ads campaign performance dashboard (BTA - Build the Agent) that visualizes data from Google Sheets.

### Data Flow Architecture
1. **Google Ads Scripts** (`scripts/`) extract data from Google Ads accounts
2. **Google Apps Script** (`scripts/deploy-sheet.js`) exposes Sheet data as JSON API
3. **Next.js app** fetches from this API and displays visualizations

### Key Directories
- `src/app/` - Next.js 15 App Router pages and API routes
- `src/components/` - Reusable React components with shadcn/ui
- `src/lib/` - Core business logic, types, and data fetching
- `scripts/` - Google Ads Scripts and deployment utilities
- `docs/` - Developer documentation for adding new data sources

### Core Data Types
The app handles three main data types from Google Sheets:
- **Daily Campaign Data** - Campaign performance by date (AdMetric)
- **Search Terms Data** - Search term performance (SearchTermMetric) 
- **Ad Groups Data** - Ad group performance (AdGroupMetric)

### State Management
- Uses React Context (`SettingsContext`) for global settings and data
- SWR for data fetching with caching
- No external state management library

### Data Processing Pipeline
- `src/lib/sheetsData.ts` - Fetches and parses data from Google Sheets API
- `src/lib/types.ts` - TypeScript interfaces for all data structures
- `src/lib/config.ts` - Configuration for sheet tabs and metric formatting 
- `src/lib/metrics.ts` - Calculated metrics and business logic

## Google Ads Metric Conventions

**Primary Metrics (use these exact names):**
- `impr` for Impressions
- `clicks` for Total Clicks  
- `cost` for Total Cost
- `conv` for Conversions
- `value` for Conversion Value

**Calculated Metrics:**
- `CPC` for Cost per Click
- `CTR` for Click-Through Rate
- `CvR` for Conversion Rate
- `ROAS` for Return on Ad Spend
- `CPA` for Cost per Acquisition

## UI Component Guidelines

- Uses **shadcn/ui** components from `src/components/ui/`
- **Tremor React** for charts and data visualizations
- **Tailwind CSS** for styling
- **Framer Motion** for animations
- Keep components focused and reusable

## Adding New Data Sources

When adding new data types, follow this sequence:
1. Update Google Ads script with camelCase headers matching Next.js expectations
2. Add TypeScript interface to `src/lib/types.ts`
3. Update `SHEET_TABS` and `TAB_CONFIGS` in `src/lib/config.ts`
4. Create fetch function in `src/lib/sheetsData.ts`
5. Build new page following existing patterns

See `docs/adding-new-data.md` for complete implementation guide.

## Code Style Requirements

- Use **camelCase** for all Google Sheet headers (critical for data pipeline)
- Ensure proper TypeScript typing for all files
- Follow Next.js 13+ App Router conventions
- Parse numeric fields with `Number()` or `parseFloat()`
- Format percentages with 0 decimal places
- Handle all API errors gracefully with user feedback

## Authentication & Data Source

The app connects to Google Sheets via a deployed Google Apps Script web app. The sheet URL is configurable through the settings page (`src/app/settings/page.tsx`).