# React Redux_Toolkit Typescript Project setup with Shadcn Library

1. Create a vite project with react and typescript

```npm
npm create vite
```

2. install typescript

```npm
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

    2.1 change the config file and add the tailwind drives @...
    2.2

```json

export default {
content: [
"./index.html",
"./src/**/*.{js,ts,jsx,tsx}",
],
theme: {
extend: {},
},
plugins: [],
}

```

3. install `Redux toolkit` and wrap the `main.ts` file with Provider

```npm
npm install @reduxjs/toolkit react-redux
```

4. install shahCn UI library for tailwind
   [installations link](https://ui.shadcn.com/docs/installation/vite)
