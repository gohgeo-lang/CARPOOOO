# Next.js Conversion Notes

This document captures general guidance for migrating a static HTML/CSS + Bootstrap project to Next.js with Tailwind CSS and React Icons.

## 1. Plan the migration
- Audit existing pages (e.g., `index.html`, `booking.html`, etc.) and identify shared elements such as navigation bars, footers, and components.
- Map each HTML page to a route within Next.js. For a pages-based structure, `pages/index.tsx` replaces `index.html`, while nested routes can be represented as directories (`pages/booking/index.tsx`).
- Decide on incremental vs. full rewrite. You can migrate page by page while keeping legacy assets available until the entire project is ported.

## 2. Initialize Next.js with Tailwind CSS
1. Create a new Next.js app:
   ```bash
   npx create-next-app@latest
   ```
2. Follow the prompts to choose TypeScript or JavaScript.
3. Install and configure Tailwind CSS following the official docs:
   ```bash
   cd <project>
   npm install -D tailwindcss postcss autoprefixer
   npx tailwindcss init -p
   ```
4. Update `tailwind.config.js` `content` paths to include the directories where you will place components and pages.
5. Add Tailwind directives to `styles/globals.css`:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

## 3. Transform layout and styles
- Convert global styles from `style.css` or other CSS files into Tailwind utility classes. For complex reusable styles, define Tailwind components or use `@layer components` in a CSS file.
- Replace Bootstrap layout classes with Tailwind equivalents (`flex`, `grid`, `container`, `mx-auto`, etc.).
- Keep legacy CSS modules for one-off styles during migration; remove them once all utilities are ported.

## 4. Rebuild components in React
- For each HTML fragment (navbars, cards, forms), create a React component under `components/`. Break large pages into smaller reusable components to simplify maintenance.
- Convert inline scripts or DOM manipulations into React state/effect logic.
- Replace `<link>` and `<script>` tags with `Head` component usage when necessary (`next/head`).

## 5. Introduce React Icons
- Install the package: `npm install react-icons`.
- Replace icon `<i class="bi bi-*"></i>` markup with React Icon components, e.g.:
  ```tsx
  import { FaCar } from 'react-icons/fa';

  <FaCar className="text-xl text-blue-500" />
  ```
- Use Tailwind utility classes to control size, color, and spacing.

## 6. Handle assets and images
- Move images from the `img/` directory into `public/` in the Next.js project.
- Use the `<Image>` component from `next/image` for optimized loading where possible.

## 7. Routing and navigation
- Replace `<a href="...">` tags that navigate within the site with Next.js `<Link>` components:
  ```tsx
  import Link from 'next/link';

  <Link href="/booking" className="text-primary">Book now</Link>
  ```
- For dynamic routes (e.g., car detail pages), use file-system routing with square brackets (`pages/cars/[id].tsx`).

## 8. Form handling and interactivity
- Convert plain forms to controlled components or use form libraries (e.g., React Hook Form) if needed.
- Implement client-side validation using React state or integrate with APIs via Next.js API routes.

## 9. Testing and verification
- Run `npm run dev` to test pages during migration.
- Use ESLint and TypeScript (if enabled) to catch errors early.
- Compare visual output with the original site. Tailwind's utility-first approach can replicate Bootstrap layouts without errors when applied consistently.

## 10. Deployment considerations
- Configure environment variables via `.env.local` for API keys.
- Deploy with Vercel or another host once the build passes: `npm run build` and `npm run start`.

### Can the migration be error-free?
Yes. Migrating from static HTML/Bootstrap to Next.js with Tailwind CSS and React Icons is feasible without runtime errors if you proceed incrementally, test frequently, and ensure each component is ported correctly. The key is thorough planning, modular component design, and leveraging Next.js tooling (TypeScript, ESLint) to catch issues early.

