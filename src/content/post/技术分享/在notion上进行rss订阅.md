---
date: 2024/10/21
---
<aside> 👨‍💻 Source code is [available here](https://github.com/antoinechalifour/notion-embed-rss).

</aside>

---

<aside> 💪 **Public roadmap** _Please file an issue on the [Github repository](https://github.com/antoinechalifour/notion-embed-rss/issues) for any feature request._

</aside>

- [x] Customise the time limit (currently 14 days)
- [ ] Search an article
- [ ] "_Show older articles_"

---

<aside> ❓ **How to use this Widget**

</aside>

1. Gather all the RSS feeds URLs ;
2. Replace the **[COMA_SEPARATED_SOURCES]** section of the following URL with the concatenated feeds URLs (coma-separated) ;
3. (Option) Tell the widget how many days since the articles have been published (defaults to 14)

```
<https://notion-widget-rss.vercel.app/?sources=**[COMA_SEPARATED_SOURCES]**&sinceDays=**[NUMBER]**>
```

3. Create an _Embed_ block and paste the URL generated in step 2.

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/4ea7cd5f-065e-4eb2-8cd8-35a4032144ca/452604e5-df89-42b6-9d5e-7fb0d92778dc/Untitled.png)

4. You're done!

---

<aside> ℹ️ **Show me an example**

</aside>

Let's say I want to generate a feed using the following sources :

- [https://www.reddit.com/r/Notion.rss](https://www.reddit.com/r/Notion.rss)
- [https://www.reddit.com/r/javascript.rss](https://www.reddit.com/r/javascript.rss)

The URL I need to copy is

```
<https://notion-widget-rss.vercel.app/?sources=**https://www.reddit.com/r/Notion.rss**,**https://www.reddit.com/r/javascript.rss**>
```

[https://notion-widget-rss.vercel.app/?sources=https://www.reddit.com/r/Notion.rss,https://www.reddit.com/r/javascript.rss](https://notion-widget-rss.vercel.app/?sources=https://www.reddit.com/r/Notion.rss,https://www.reddit.com/r/javascript.rss)

---

<aside> 💅 **Customise**

</aside>

This widget is customisable so that it can match your Notion pages theme. You use the following `query` params to customise :

- The _font_:
    - mono: `&font=mono` ;
    - serif: `&font=serif` ;
    - default (sans-serif): `&font=default`
- The _theme:_
    - light: `&theme=light` ;
    - dark: `&theme=dark`

_Examples :_

**Serif with Light Theme**

```
<https://notion-widget-rss.vercel.app/?**font=serif&theme=light&**sources=https://www.reddit.com/r/webdev.rss>
```

[https://notion-widget-rss.vercel.app/?sources=https://www.reddit.com/r/webdev.rss&font=serif&theme=light](https://notion-widget-rss.vercel.app/?sources=https://www.reddit.com/r/webdev.rss&font=serif&theme=light)

**Mono with Dark Theme**

```
<https://notion-widget-rss.vercel.app/?**font=mono&theme=dark&**sources=https://www.reddit.com/r/webdev.rss>
```

[https://notion-widget-rss.vercel.app/?sources=https://www.reddit.com/r/webdev.rss&font=mono&theme=dark](https://notion-widget-rss.vercel.app/?sources=https://www.reddit.com/r/webdev.rss&font=mono&theme=dark)

---

<aside> 💅 **Troubleshooting**

</aside>

⛔️ **Invalid source or feed**

In the example bellow, **[http://example.com](http://example.com)** and [**](http://localhost/feed)[http://localhost/feed**](http://localhost/feed**) are invalid sources.

[https://notion-widget-rss.vercel.app/?sources=http://example.com,http://localhost/feed,https://www.reddit.com/r/webdev.rss&font=mono](https://notion-widget-rss.vercel.app/?sources=http://example.com,http://localhost/feed,https://www.reddit.com/r/webdev.rss&font=mono)

If the widget cannot fetch or parse the RSS feed, an error message will be displayed in place. To fix it, verify the source or remove it.If you're sure the URL and the RSS feed is valid, please [file an issue on the Github repository](https://github.com/antoinechalifour/notion-embed-rss/issues).