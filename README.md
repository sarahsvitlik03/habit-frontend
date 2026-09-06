# Chore Tracker

A lightweight **Vue 3 + Vite** application built as the frontend for a habit‑tracking platform. This project focuses on clean UI architecture, fast builds, and a simple deployment workflow using **GitHub Pages**.

## Features

- Vue 3 Composition API for modular, maintainable components  
- Vite 7 for instant HMR and optimized production builds  
- Reusable UI components for habit cards, progress indicators, and daily check‑ins  
- GitHub Pages deployment using a `/docs` build directory  
- ESLint + Vue recommended rules for consistent code quality  

## Tech Stack

- Vue 3  
- Vite 7  
- JavaScript  
- HTML  
- ESLint (Vue + JS rules)

## Getting Started

### Install dependencies
```bash
npm install
```

### Run development server
```bash
npm run dev
```

### Build for production
```bash
npm run build
```

The production build outputs to `/dist`.  
For GitHub Pages, the `/dist` folder is renamed to `/docs`.

### Lint
```bash
npm run lint
```

## Project Structure

```
habit-frontend/
├── docs/              # Production build for GitHub Pages
├── src/               # App source code
│   ├── components/    # Reusable Vue components
│   ├── assets/        # Images, icons, styles
│   └── App.vue        # Root component
├── index.html
├── vite.config.js
└── package.json
```

## Deployment

This project is deployed using **GitHub Pages**:

- **Branch:** `main`  
- **Folder:** `/docs`  

`vite.config.js` includes:

```js
base: '/habit-frontend/'
```
