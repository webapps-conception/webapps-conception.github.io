layout: post
title: 'Premiers pas sur Next.js'
date: '2022-11-15T17:00:00.000Z'
tags: react next.js

# Modèles de sites web
* <https://vercel.com/new/templates>

# Créer un blog
## Introduction

Il s'agit du [blog-starter](https://github.com/vercel/next.js/tree/canary/examples/blog-starter){:target="_blank"} plus TypeScript existant.

Ce blog présente la fonctionnalité de [génération statique](https://nextjs.org/docs/basic-features/pages){:target="_blank"} de Next.js en utilisant des fichiers Markdown comme source de données.

Les articles de blog sont stockés dans /_posts en tant que fichiers Markdown avec prise en charge des informations préliminaires. L'ajout d'un nouveau fichier Markdown créera un nouveau billet de blog.

Pour créer les articles de blog, nous utilisons [remark](https://github.com/remarkjs/remark){:target="_blank"} et [remark-html](https://github.com/remarkjs/remark-html){:target="_blank"} pour convertir les fichiers Markdown en une chaîne HTML, puis l'envoyons comme accessoire à la page. Les métadonnées de chaque publication sont gérées par [gray-matter](https://github.com/jonschlinkert/gray-matter){:target="_blank"} et également envoyées en accessoires à la page.

## Démo
* <https://next-blog-starter.vercel.app/>

## Génération d'un blog Next.js

Si vous souhaitez publier et commenter vos codes source sur un blog, next.js permet de générer un modèle de blog en une simple commande.

```shell
yarn create next-app --example blog-starter blog-starter-app
```

## Générer un site web avec tailwindcss

Cet exemple montre comment utiliser [Tailwind CSS](https://tailwindcss.com/){:target="_blank"} avec Next.js. Il suit les étapes décrites dans les [documents officiels de Tailwind](https://tailwindcss.com/docs/guides/nextjs){:target="_blank"}.

```shell
yarn create next-app --example with-tailwindcss with-tailwindcss-app
```

* <https://github.com/vercel/next.js/tree/canary/examples/with-tailwindcss>{:target="_blank"}

## Compiler Markdown avec la coloration syntaxique
Pour une meilleure lisibilité, nous devons mettre en évidence des groupes de texte spécifiques.

Heureusement, il existe une jolie petite bibliothèque qui le fait déjà pour nous, highlight.js.

Markdown-it a une option en surbrillance que nous pouvons facilement utiliser pour nous connecter.

Ainsi, il faut remplacer les composants ``remark`` et ``remark-html`` par ``highlight.js`` et ``markdown-it`` :
```bash
yarn remove remark remark-html
yarn add highlight.js markdown-it
```

Remplacer le fichier lib/markdownToHtml.ts par les lignes suivantes :
```tsx
import hljs from 'highlight.js'
import Markdown from 'markdown-it'

export default async function markdownToHtml(markdown: string) {
  const md = Markdown({
    highlight: (
      str: string,
      lang: string,
    ) => {
      const code = lang && hljs.getLanguage(lang)
        ? hljs.highlight(str, {
            language: lang,
            ignoreIllegals: true,
          }).value
        : md.utils.escapeHtml(str);
      return `<pre class="hljs"><code>${code}</code></pre>`;
    },
  })

  const result = md.render(markdown)
  return result.toString()
}
```

Ajout du style CSS dans le fichier pages/_app.tsx :
```tsx
import 'highlight.js/styles/stackoverflow-dark.css'
```

* <https://jasonvan.ca/posts/compile-markdown-to-html-using-node-js-with-syntax-highlighting>{:target="_blank"}

## Note
Ce blog utilise le framework [Tailwind CSS](https://tailwindcss.com/){:target="_blank"}, et peut être déployé facilement sur le cloud [Vercel](https://vercel.com/new?utm_source=github&utm_medium=readme&utm_campaign=next-example){:target="_blank"} ([Documentation](https://nextjs.org/docs/deployment){:target="_blank"}.

* <https://github.com/vercel/next.js/tree/canary/examples/blog-starter>{:target="_blank"}
