# Cy's Grimoire

This is a comprehensive web tool for the mobile game 🎲 "Toram Online" developed by [Asobimo, Inc.](https://asobimo.com/). It provides various utilities and calculators to enhance your gaming experience, including character simulation, damage calculation, equipment management, and more.

⭐ Deployed on [here](https://cy-grimoire.netlify.app/).

- ☕ Author: Cyteria & Aul
- 📧 Email: cyteria39@gmail.com

## Project Overview

Cy's Grimoire is a Vue.js-based web application designed to assist Toram Online players with in-game calculations, simulations, and data management. It offers tools for optimizing character builds, calculating damage, managing enchantments, and querying game data like skills, items, and quests.

## Features

- **Character Simulator**: Simulate and optimize character builds, including equipment, skills, and stats.
- **Damage Calculation**: Calculate damage output for various attacks and scenarios.
- **Enchant System**: Manage and simulate equipment enchantments.
- **Item Database**: Browse and search through game items.
- **Skill Query**: Look up and analyze skills.
- **Quest Information**: Access quest details and requirements.
- **Registlet Management**: Handle registlet builds and effects.
- **Multi-language Support**: Available in English, Japanese, Simplified Chinese, and Traditional Chinese.
- **Responsive Design**: Optimized for desktop and mobile devices.

## Functional Requirements

The following are the key functional requirements of Cy's Grimoire:

- **Character Simulation**: The system shall allow users to create and simulate character builds, including equipment, skills, and stat calculations.
- **Damage Calculation**: The system shall provide tools to calculate damage output for various attack types and scenarios.
- **Enchantment Management**: The system shall enable users to manage and simulate equipment enchantments and their effects.
- **Item Database Query**: The system shall allow browsing, searching, and filtering of game items.
- **Skill Information Access**: The system shall provide detailed information and analysis of character skills.
- **Quest Data Retrieval**: The system shall display quest details, requirements, and rewards.
- **Registlet Build Management**: The system shall support creating and managing registlet builds and their effects.
- **Multi-language Support**: The system shall support multiple languages (English, Japanese, Simplified Chinese, Traditional Chinese) for user interface and content.

## Non-Functional Requirements

The following are the key non-functional requirements of Cy's Grimoire:

- **Performance**: The application shall load within 3 seconds on standard internet connections and respond to user interactions within 100ms.
- **Usability**: The interface shall be intuitive and accessible, with clear navigation and responsive design for desktop and mobile devices.
- **Reliability**: The system shall maintain 99% uptime and handle errors gracefully without data loss.
- **Scalability**: The application shall support a growing number of users and data without significant performance degradation.
- **Security**: User data shall be handled securely, with no sensitive information stored client-side.
- **Compatibility**: The system shall work on modern browsers (Chrome, Firefox, Safari, Edge) and mobile devices.
- **Maintainability**: Code shall be well-structured, documented, and follow best practices for easy updates and bug fixes.

## Technologies Used

Cy's Grimoire is built using the following technologies:

- **Frontend Framework**: Vue.js 3 (Composition API)
- **Build Tool**: Vite
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **State Management**: Pinia
- **Routing**: Vue Router 4
- **Internationalization**: Vue I18n
- **Icons**: Iconify
- **Charts/Data Visualization**: (If applicable, based on dependencies)
- **Other Libraries**:
  - Marked (Markdown parsing)
  - PapaParse (CSV parsing)
  - DOMPurify (HTML sanitization)
  - Velocity Animate (Animations)
  - Vue Draggable (Drag-and-drop functionality)
- **Development Tools**:
  - ESLint (Code linting)
  - Prettier (Code formatting)
  - TypeScript Compiler (Type checking)
  - Vite Plugin PWA (Progressive Web App support)

## Folder Structure

```
Toram_Grimoire2/
├── public/                 # Static assets (favicons, images, robots.txt)
├── src/
│   ├── app/                # Application initialization files
│   ├── assets/             # CSS, images, and other assets
│   ├── components/         # Reusable Vue components
│   │   ├── app-layout/     # Layout components
│   │   ├── card/           # Card-related components
│   │   ├── common/         # Common utility components
│   │   ├── cyteria/        # Custom UI components
│   │   └── dev/            # Development-related components
│   ├── lib/                # Core logic and data structures
│   │   ├── Character/      # Character-related calculations
│   │   ├── Damage/         # Damage calculation logic
│   │   ├── Enchant/        # Enchantment system
│   │   ├── Enemy/          # Enemy data
│   │   ├── Items/          # Item management
│   │   ├── Quest/          # Quest data
│   │   ├── Registlet/      # Registlet builds
│   │   └── Skill/          # Skill data
│   ├── locales/            # Internationalization files
│   ├── plugin/             # Vue plugins
│   ├── router/             # Vue Router configuration
│   ├── shared/             # Shared utilities and constants
│   ├── stores/             # Pinia state management
│   └── views/              # Page components
├── .gitignore              # Git ignore rules
├── package.json            # Project dependencies and scripts
├── vite.config.ts          # Vite configuration
├── tailwind.config.ts      # Tailwind CSS configuration
├── tsconfig.json           # TypeScript configuration
└── README.md               # This file
```

## Local Setup

To run this project locally, follow these steps:

### Prerequisites

- Node.js (version 16 or higher)
- Yarn (recommended) or npm

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/Toram_Grimoire2.git
   cd Toram_Grimoire2
   ```

2. Install dependencies:
   ```bash
   yarn install
   # or
   npm install
   ```

3. Start the development server:
   ```bash
   yarn dev
   # or
   npm run dev
   ```

4. Open your browser and navigate to `http://localhost:5173` (or the port shown in the terminal).

### Available Scripts

- `yarn dev` / `npm run dev`: Start the development server
- `yarn build` / `npm run build`: Build the project for production
- `yarn preview` / `npm run preview`: Preview the production build locally
- `yarn lint` / `npm run lint`: Run ESLint to check and fix code style
- `yarn type-check` / `npm run type-check`: Run TypeScript type checking
- `yarn pretty` / `npm run pretty`: Format code with Prettier

## Deployment to Netlify

This project is configured for easy deployment to Netlify. Follow these steps:

1. **Build the project**:
   ```bash
   yarn build
   # or
   npm run build
   ```

2. **Deploy to Netlify**:
   - Connect your GitHub repository to Netlify
   - Set the build command to: `yarn build` (or `npm run build`)
   - Set the publish directory to: `dist`
   - Add any necessary environment variables in Netlify's dashboard

3. **Alternative: Manual deployment**:
   - Upload the `dist` folder contents to your Netlify site
   - Or use Netlify CLI: `netlify deploy --prod --dir=dist`

The site will be automatically rebuilt on each push to the main branch if connected to GitHub.

## Contributing

We welcome contributions to Cy's Grimoire! To contribute:

1. Fork the repository
2. Create a new branch for your feature: `git checkout -b feature/your-feature-name`
3. Make your changes and commit them: `git commit -m 'Add some feature'`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

### Guidelines

- Follow the existing code style (ESLint and Prettier configurations)
- Write clear, concise commit messages
- Test your changes thoroughly
- Update documentation if necessary
- Respect the project's multi-language support

## Screenshots and Documentation

### Screenshots

*(Add screenshots here)*

- Home Page
- Character Simulator
- Damage Calculator
- Item Database

### Documentation

For more detailed information:

- [Vue.js Documentation](https://vuejs.org/)
- [Vite Documentation](https://vitejs.dev/)
- [Toram Online Official Site](https://asobimo.com/)
- [Netlify Deployment Guide](https://docs.netlify.com/)

---

If you have any questions or need help, feel free to open an issue or contact the author.
