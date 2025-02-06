# Material UI v6 + Tailwind CSS v4

## Next.js

```bash
pnpm create next-app mui6-tailwind4-nextjs
```

## Tailwind v4

```bash
npx @tailwindcss/upgrade@next
```

## Material UI

```bash
pnpm install @emotion/styled @emotion/react @emotion/cache @mui/material @mui/material-nextjs @mui/icons-material
```

## CSS layer

Follow [Material UI Next.js App Router integration](https://mui.com/material-ui/integrations/nextjs/) and add `options={{ enableCssLayer: true }}` prop.

Insert `mui` layer in between tailwind layers:

```diff
diff --git a/src/app/globals.css b/src/app/globals.css
index 4040e97..aac0a5f 100644
--- a/src/app/globals.css
+++ b/src/app/globals.css
@@ -1,4 +1,7 @@
-@import 'tailwindcss';
+/* `mui` comes from `enableCssLayer: true` */
+/* need to insert `mui` in between so that Tailwind utility classes can override Material UI components */
+@layer theme, base, mui, components, utilities;
+@import "tailwindcss";

 @theme {
   --color-background: var(--background);

```

## Dark mode

Follow [Tailwind docs](https://tailwindcss.com/docs/dark-mode#toggling-dark-mode-manually) and update Material UI to use `.dark` selector.

```diff
diff --git a/src/index.css b/src/index.css
index f1d8c73..9c0f814 100644
--- a/src/index.css
+++ b/src/index.css
@@ -1 +1,3 @@
 @import "tailwindcss";
+
+@custom-variant dark (&:where(.dark, .dark *));

```

```diff
diff --git a/src/App.tsx b/src/App.tsx
index 154635b..2dad18b 100644
--- a/src/App.tsx
+++ b/src/App.tsx
@@ -48,6 +48,7 @@ function AppContent() {

   return (
     <Box
+      className="bg-gray-50 dark:bg-gray-900"
       sx={{
         bgcolor: "background.default",
         color: "text.primary",
@@ -112,6 +113,9 @@ function AppContent() {

 function App() {
   const theme = createTheme({
+    cssVariables: {
+      colorSchemeSelector: "class",
+    },
     colorSchemes: {
       light: true,
       dark: true,
```

## Tailwind Intellisense

To enable autocompletion when using `className: "..."`, open the VS Code `settings.json` and add this

```json
{
  "tailwindCSS.experimental.classRegex": [
    ["className\\s*:\\s*['\"]([^'\"]*)['\"]"]
  ]
}
```
