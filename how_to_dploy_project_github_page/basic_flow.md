## তোমার বর্তমান next.config.ts:
```
1. next.config.ts Update করো

ধরো তোমার GitHub repository নাম:

portfolio

তাহলে:

import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  output: "export",

  basePath: "/portfolio",
  assetPrefix: "/portfolio",

  images: {
    unoptimized: true,
  },
};

export default nextConfig;
```
### Portfolio-এর জন্য Best Practice
```
তোমার project structure যদি এমন হয়:

app/
 ├── dashboard/
 │   └── page.tsx
 └── page.tsx

তাহলে redirect না দিয়ে root page থেকেই dashboard component render করা ভালো।

app/page.tsx
import DashboardComponents from "@/components/dashboard/DashboardComponents";

export default function Home() {
  return <DashboardComponents />;
}
এতে URL হবে:

https://your-domain.com/

3. Root page তৈরি করো

যদি তোমার portfolio page app/dashboard/page.tsx এ থাকে, তাহলে app/page.tsx তৈরি করো:

import { redirect } from "next/navigation";

export default function Home() {
  redirect("/dashboard");
}

4. Build Test

npm run build
GitHub Push
git add .
git commit -m "portfolio ready for deployment"
git push origin main

4. GitHub Actions Deploy

Project root-এ file তৈরি করো:

.github/workflows/deploy.yml
<img width="1069" height="486" alt="image" src="https://github.com/user-attachments/assets/59aa8ff9-b4a1-4a08-99ce-59a2cb2e879f" />


Content:

name: Deploy Next.js to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - run: npm install
      - run: npm run build

      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./out

  deploy:
    needs: build

    runs-on: ubuntu-latest

    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
5. GitHub Pages Enable

GitHub Repository →

Settings → Pages

Source:

GitHub Actions
 or yml file hobe
name: Deploy Next.js Portfolio to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm install

      - name: Build project
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./out

  deploy:
    needs: build
    runs-on: ubuntu-latest

    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
