# Technical Design Document (TDD)

## 1. Overview

This **Technical Design Document (TDD)** outlines the architectural, technological, and deployment strategies for the Bartwo Company Website. It provides detailed specifications on how the site will be built, configured, tested, and deployed as a **Static Site Generation (SSG)** project. The goal is to ensure high performance, maintainability, and scalability within a streamlined development workflow.

---

## 2. Scope and Objectives

1. **Scope**  
   - Build a modern, high-performance, and interactive website using **Nuxt 3** (configured for SSG).
   - Serve as both a **corporate identity** and **portfolio showcase** with unique design elements.
   - Deploy to a **cPanel** environment for ease of hosting and management.

2. **Objectives**  
   - Deliver a **lightweight** but feature-rich codebase, minimizing dependencies.
   - Integrate dynamic, interactive features (3D/animation) without compromising performance.
   - Maintain a robust **CI/CD pipeline** (where feasible) to automate build and deployment steps.
   - Uphold **best practices** for security, SEO, and accessibility.

---

## 3. Architecture and Technology Stack

### 3.1 High-Level Architecture
┌───────────────────────┐
│     Local Dev Env     │
│ Nuxt + Node + Tools   │
└───────────┬───────────┘
│
▼
┌──────────────────────┐
│  Static Site Output  │  (Generated via Nuxt SSG)
│     (HTML/CSS/JS)    │
└───────────┬──────────┘
│
▼
┌──────────────────────┐
│      cPanel Host     │
│ (Static Asset Serve) │
└──────────────────────┘

1. **Development Layer**  
   - Nuxt 3 (with SSG mode) runs in a local environment.  
   - Developers utilize Node.js, front-end libraries, and bundlers for building, testing, and previewing.

2. **Build Layer**  
   - **Nuxt Generate** process outputs static HTML/CSS/JS.  
   - Generated assets are placed in a folder (e.g., `dist/`).

3. **Deployment Layer**  
   - Static assets are uploaded to **cPanel** hosting, served via Apache or Nginx (depending on cPanel’s config).  
   - **No Node.js runtime** needed on the server; it serves only static files.

### 3.2 Tech Stack Components

| **Component**                | **Description**                                                                            |
|----------------------------- |--------------------------------------------------------------------------------------------|
| **Nuxt 3**                   | Main framework for SSR/SSG. Provides file-based routing, modular structure, and SEO edge. |
| **Vue 3**                    | Core UI engine leveraged by Nuxt for component-driven architecture.                       |
| **Tailwind CSS** (or SCSS)   | Utility-first or custom SCSS for styling, with emphasis on flat design principles.        |
| **GSAP** / **Three.js** / **Zdog** | Libraries for advanced animations, 3D elements, and interactive features.              |
| **Nuxt Content** (optional)  | For managing blog or dynamic data via Markdown within the Nuxt ecosystem.                |
| **ESLint / Prettier**        | Code linting and formatting tools for consistent code quality.                            |

---

## 4. Detailed Implementation

### 4.1 Directory Structure

Below is an example directory structure for a **Nuxt 3** project configured for SSG:
bartwo-website/
├─ .nuxt/               # Auto-generated folder (ignored in version control)
├─ node_modules/
├─ public/              # Public static assets
├─ assets/              # Images, fonts, global SCSS/Tailwind configs
├─ components/          # Reusable Vue components
├─ composables/         # Reusable logic (optional in Nuxt 3)
├─ layouts/             # Reusable layouts (header/footer)
├─ pages/               # Page components (file-based routing)
│  ├─ index.vue         # Home page
│  ├─ about.vue         # About page
│  ├─ services.vue      # Services page
│  ├─ portfolio.vue     # Portfolio page
│  ├─ blog.vue          # Blog listing page
│  └─ contact.vue       # Contact page
├─ plugins/             # Nuxt/Vue plugins (e.g., GSAP, Three.js)
├─ store/               # Pinia/Vuex store (if needed)
├─ content/             # (Optional) Blog/Markdown content for Nuxt Content
├─ nuxt.config.ts       # Nuxt configuration (includes SSG settings)
├─ package.json         # Project dependencies/scripts
├─ README.md            # Project documentation
└─ tsconfig.json        # TypeScript config (if using TS)

### 4.2 Key Configuration Files

1. **`nuxt.config.ts`**  
   - **Mode**: `ssr: false` or using `target: 'static'` for SSG.  
   - **Router**: Provide base URL or custom routing if needed.  
   - **Build**: Configure optimizations, minification, and bundling.  
   - **Modules**: Integrate required modules (`@nuxtjs/tailwindcss`, `nuxt-graphql-client`, `@nuxt/content`, etc.).  
   - **Runtime Config**: Securely store environment variables.

2. **`package.json`**  
   - Custom scripts:  
     - `dev`: for local development  
     - `build`: for building the project  
     - `generate`: for generating static files  
     - `preview`: for local previews of the static output

### 4.3 Interactive Features & Animations

- **GSAP** for choreographed animations:
  - **ScrollTrigger**: to tie animations to scroll positions (e.g., parallax).
  - **Timelines**: for complex, sequence-based interactions.
- **Three.js** or **Zdog** for 3D hero section:
  - Lightweight scenes or simple shapes that react to user input (hover, scroll).
- **Micro-Interactions**:
  - Button hover transitions, subtle content reveals, or link hover expansions.

### 4.4 Performance and Optimizations

1. **Asset Optimization**:  
   - Use **Nuxt Image** or a manual approach to handle responsive images with WebP/AVIF.  
   - Minimize large JavaScript libraries; tree-shake unused code.

2. **Lazy Loading**:  
   - Dynamically import components not crucial to initial render.  
   - Utilize `v-lazy` for images or built-in Nuxt features.

3. **Code Splitting**:  
   - Nuxt automatically code-splits pages and components.  
   - Evaluate critical vs. non-critical scripts to ensure faster initial load.

4. **Caching & CDN**:  
   - Enable **browser caching** for static assets.  
   - Optionally serve from a CDN if feasible (though standard cPanel might limit direct CDN integration).

### 4.5 Security Considerations

- **HTML Sanitization** for any user input (forms, blog comments).  
- **reCAPTCHA** or similar anti-spam solutions for the contact form.  
- **HTTPS**: Ensure the final domain has a valid SSL certificate.  
- **Content Security Policy (CSP)**: Enforce strict CSP headers to reduce XSS risks.

---

## 5. Development Workflow

1. **Local Development**  
   - Run `npm install` to install dependencies.  
   - Use `npm run dev` to start the local dev server.  
   - Hot-reloading enabled for `.vue` components and `.js`/`.ts` files.

2. **Git Version Control**  
   - **Feature Branching**: Create separate branches for each feature or bug fix.  
   - **Pull Requests (PRs)**: Code reviews before merging into `main` or `develop`.

3. **Testing & QA**  
   - **Unit Tests**: (Optional) with **Jest** or **Vitest** for core components.  
   - **End-to-End Tests**: (Optional) with **Cypress** for critical user flows.  
   - **Accessibility Audits**: Use Lighthouse or dedicated tools to ensure WCAG compliance.

4. **Build & Generate**  
   - **Static Generation**: `npm run generate` to output the fully static site to the `dist/` folder.

---

## 6. Deployment to cPanel

### 6.1 Build Artifacts

1. **Local**: After running `npm run generate`, the `dist/` folder will contain all static files.  
2. **Testing**: Verify the static site locally or on a staging environment (if available).  
3. **Final Artifact**: Zip the `dist/` folder or directly upload its contents.

### 6.2 Deployment Steps

1. **cPanel File Manager**:  
   - Log in to cPanel.  
   - Navigate to `public_html/` or your chosen folder (e.g., `bartwo/`).  
   - Delete or backup any old site files to avoid conflicts.

2. **Upload Static Files**:  
   - Upload the entire `dist/` folder contents.  
   - Rename `dist/` to match the root folder or place the contents directly in `public_html/`.

3. **Configuration**:  
   - If the site is deployed in a subdirectory (e.g., `example.com/bartwo`), ensure `router.base` in `nuxt.config.ts` matches (e.g., `base: '/bartwo/'`).  
   - Confirm `.htaccess` rules to avoid 404 errors on direct page loads.

4. **SSL and Domain Setup**:  
   - Enable **SSL** if available.  
   - Update domain settings to point to the correct folder in **cPanel** (e.g., using **Addon Domains** or **Subdomains**).

5. **Validation**:  
   - Test the live URL for correct rendering, routing, and animations.  
   - Check logs or cPanel error pages for missing files or MIME type errors.

---

## 7. Environment Variables and Configuration

- **.env File**: Store any secrets or API keys (if needed) locally.  
- **Public vs Private**: Use `NUXT_PUBLIC_*` variables for front-end usage, while sensitive keys remain server-side (though for SSG, limited usage is typical).  
- **Runtime Config**: Expose environment variables via `nuxt.config.ts` if dynamic content or external APIs are used.

---

## 8. Testing and Quality Assurance

1. **Performance Testing**  
   - **Lighthouse**: Aim for a 90+ performance score.  
   - **PageSpeed Insights**: Identify opportunities for further optimization.

2. **Accessibility Testing**  
   - **Keyboard Navigation**: Ensure all interactive elements are reachable.  
   - **ARIA Labels**: Provide meaningful descriptions for screen readers.  
   - **Color Contrast**: Check that text is legible against the background.

3. **Cross-Browser Testing**  
   - Test in **Chrome, Firefox, Safari, Edge** at minimum.  
   - Verify animations degrade gracefully in older browsers.

4. **Responsive Testing**  
   - Evaluate breakpoints on mobile, tablet, and desktop.  
   - Confirm interactive elements remain functional at various screen sizes.

---

## 9. Risk Management

| **Risk**                                | **Mitigation Strategy**                                       |
|----------------------------------------|---------------------------------------------------------------|
| **High Animation Overhead**            | Use **requestAnimationFrame**, optimize 3D libraries, ensure minimal CPU/GPU usage. |
| **Large Bundle Size**                  | Employ **code splitting**, only import needed library chunks. |
| **Deployment Issues on cPanel**        | Thoroughly **test locally** and on a staging environment before final deployment. |
| **SEO Limitations**                    | Pre-render all content with Nuxt SSG, add **meta tags** and **structured data**. |
| **Security Breaches**                  | Enforce **HTTPS**, sanitize user input, implement **CSP** and secure headers. |

---

## 10. Timeline & Milestones

| **Milestone**             | **Details**                                           | **Timeline**   |
|---------------------------|-------------------------------------------------------|----------------|
| **Technical Planning**    | Confirm architecture, finalize stack choices.        | Week 1         |
| **Setup & Configuration** | Initialize Nuxt 3, Tailwind, and plugin integration. | Week 2         |
| **Core Feature Dev**      | Implement 3D hero, interactions, main pages.         | Week 3-4       |
| **Content Integration**   | Set up blog (Nuxt Content), about, portfolio pages.  | Week 4-5       |
| **Testing & Optimization**| Performance tuning, QA, and A11y checks.             | Week 6         |
| **SSG Build**            | Generate static site; verify local output.           | Week 7         |
| **Deployment & Launch**   | Upload to cPanel; final live checks.                 | Week 8         |

*(Note: Adjust timelines as needed based on scope changes or resource availability.)*

---

## 11. Maintenance and Future Enhancements

1. **Ongoing Maintenance**  
   - **Security Patches**: Regularly update Nuxt, dependencies, and libraries.  
   - **Content Updates**: Use Nuxt Content or any headless CMS if dynamic changes become frequent.

2. **Potential Enhancements**  
   - **Progressive Web App (PWA)**: Add offline capabilities and app-like features.  
   - **Internationalization (i18n)**: Multi-language support with route-based translations.  
   - **Continuous Integration**: Automate build previews on platforms like GitHub Actions or GitLab CI.

---

## 12. Conclusion

This Technical Design Document outlines a **robust** and **innovative** approach to building Bartwo’s website via **Nuxt 3 in SSG mode**, ensuring:

- **High Performance** with static hosting.
- **Scalability** and maintainability through modular architecture.
- **Engaging Interactions** leveraging best-in-class animation and 3D libraries.
- **Ease of Deployment** to cPanel without requiring extensive server configuration.

By following these guidelines, Bartwo will have a **stand-out corporate and portfolio site** that balances memorable design with solid technical foundations.

**End of Document**  
_This TDD is subject to updates as requirements evolve or new technologies emerge._