# brettdewoody.com

Source code for my personal website at [brettdewoody.com](https://brettdewoody.com).

Built with [Astro](https://astro.build) and [Tailwind CSS v4](https://tailwindcss.com), deployed on [Netlify](https://netlify.com).

## Stack

- **Framework**: Astro v6 (static output)
- **Styling**: Tailwind CSS v4 (CSS-first config)
- **Fonts**: Orbitron + Rajdhani via Fontsource (self-hosted)
- **Content**: Markdown flat files via Astro Content Layer API
- **Hosting**: Netlify

## Local development

```sh
npm install
npm run dev
```

## Commands

| Command           | Action                                      |
| :---------------- | :------------------------------------------ |
| `npm run dev`     | Start local dev server at `localhost:4321`  |
| `npm run build`   | Build production site to `./dist/`          |
| `npm run preview` | Preview build locally before deploying      |
