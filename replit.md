# Teton Peaks View - Real Estate Landing Page

## Overview
A high-end real estate landing page for a property at 4074 S 500 W, Victor, Idaho, in Teton Valley. Features parallax scrolling, property highlights, and a contact section.

## Tech Stack
- **Framework**: React 19 with TypeScript
- **Build Tool**: Vite 7
- **Styling**: Tailwind CSS v3 + shadcn/ui components
- **UI Library**: Radix UI primitives, Lucide React icons

## Project Structure
- `Desktop/app/` — Main React/Vite application
  - `src/App.tsx` — Main landing page component
  - `src/components/ui/` — shadcn/ui component library
  - `public/` — Static assets (property images)

## Running the App
The app runs on port 5000 via the "Start application" workflow:
```
cd Desktop/app && npm run dev
```

## Development Notes
- Vite configured to use `host: 0.0.0.0`, `port: 5000`, `allowedHosts: true` for Replit proxy compatibility
- The `kimi-plugin-inspect-react` dev plugin was removed (not available in current install)
