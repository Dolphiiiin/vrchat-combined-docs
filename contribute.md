# contribute 統合ドキュメント

以下は自動収集された全ドキュメントのコンテンツです。各セクションの始めにメタデータが記載されています。



---

## ドキュメント: accessibility.md

```metadata
階層: /contribute/accessibility.md
ディレクトリ: contribute
ファイル名: accessibility.md
拡張子: .md
サイズ: 2.51 KB
最終更新: 2025-06-05T03:07:52.736Z
```

# Accessibility

import BrowserWindow from '@site/src/components/BrowserWindow';
import APITable from '@site/src/components/APITable';

You can make your documentation more accessible and inclusive by structuring it well and providing helpful metadata.

This page summarizes some key points from Google's free [Tech Writing for Accessibility](https://developers.google.com/tech-writing/accessibility/self-study) course ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)).

## Use simple language

Most users who read VRChat's Creation documentation speak English. But they may not understand complex words or phrases.

- Avoid unfamiliar jargon, US-based slang, pop-cultural references, and complicated linguistic constructions.
- Follow the [style guide](/contribute/style) to improve the clarity of your documentation.

## Create helpful headings

Markdown support different levels of headings. Headings improve the structure of your document and help your readers understand it.

- Every document should use a level-1 heading (`#`) for its title.
- Use other heading levels (`##`, `###`... ) to create sections.
- Don't skip heading levels (i.e., jumping from `##` to `#####`).

## Include informative link text

People who use screen readers often use them to scan a page to hear just the links. Use informative link text to ensure that your audience hears meaningful information, not just "Learn more, learn more, learn more."

- Avoid: [Learn more](/roadmap), [Click here](/roadmap), [This page](/roadmap)
- Better: Learn more about [our roadmap](/roadmap).

## Write helpful alt text

Markdown allows you to provide [alternative text](https://developers.google.com/tech-writing/accessibility/self-study/write-alt-text) for your images. Screen readers narrate alt text for readers who are blind or who have low vision.

Use a short phrase or one or two sentences. Long descriptions interrupt the reading flow for screen reader users.

- Don't include extra words like "Image of" or "Photo of."
- Capitalize the first word and include a final period.
- Use other punctuation as necessary.

For example:

```md
![An avatar with a backpack walks into a blue, holographic world. It's surrounded by floating objects from Unity and Udon.](/img/homepage/ill-overview.png)
```

```mdx-code-block
<BrowserWindow>
```
![An avatar with a backpack walks into a blue, holographic world with floating Unity objects and Udon Graph elements .](/img/homepage/ill-overview.png)
```mdx-code-block
</BrowserWindow>
```



---

## ドキュメント: index.md

```metadata
階層: /contribute/index.md
ディレクトリ: contribute
ファイル名: index.md
拡張子: .md
サイズ: 609 B
最終更新: 2025-06-05T03:07:52.737Z
```

---
sidebar_position: 50
---
# Contribution guide

Anyone can suggest improvements to VRChat's Creation documentation! Click "Edit this page" at the bottom of any page and follow the [instructions on GitHub](https://github.com/vrchat-community/creator-docs/blob/main/README.md). VRChat will review your changes and publish them.

These guidelines explain how you should write and structure documentation:

- [Accessibility guide](./accessibility)
- [Style guide](./style) 
- [Syntax guide](./syntax) 

:::tip

Use the "Feedback" button to suggest simple changes. We'll take care of it!

:::


---

## ドキュメント: style.md

```metadata
階層: /contribute/style.md
ディレクトリ: contribute
ファイル名: style.md
拡張子: .md
サイズ: 9.00 KB
最終更新: 2025-06-05T03:07:52.738Z
```

# Style guide

import BrowserWindow from '@site/src/components/BrowserWindow';
import APITable from '@site/src/components/APITable';

This page explains how to write clear, concise, and friendly documentation. It's a summary of Google's free [Technical Writing](https://developers.google.com/tech-writing/overview) course ([CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)) and recommends how to apply it to VRChat's Creation documentation.

You should read this page before you contribute to VRChat's creation documentation. 

:::info

VRChat's Creation documentation predates this style guide. If you want to suggest improvements to any page, please submit a [pull request](https://github.com/vrchat-community/creator-docs/pulls).

:::

## Terms and Acronyms

Unity, VRChat, and the VRChat SDK contain [terms](https://developers.google.com/tech-writing/one/words) that your reader may not be familiar with. Identify those terms when writing your documentation. 

- If you are introducing a new term, explain it. If a term already exists, provide a link to the page that explains it.
	- For example: "The 'Marketplace' tab allows you to see all VRChat worlds that use the [Creator Economy](/economy/).
- Try to use terms consistently. Don't switch between similar versions of the same term.
	- For example: "GameObject" and "game object."
- Use **lowercase** for terms that aren't unique to VRChat or the SDK, even if they mean something specific in VRChat.
	- For example: **a**vatar, **w**orld, **c**reator.
- **Capitalize** terms that are specific to VRChat or SDK, even if they include one of the words above.
	- For example: **A**vatars SDK, **W**orlds SDK, **V**RChat, **C**reator **E**conomy, **C**reator **E**conomy.

You can use acronyms for terms that are used frequently in your document. When you use an acronym in your document for the first time, you should define it. For example:  

- Software Development Kit (SDK)
- VRChat Creator Companion (VCC)
- Creator Economy (CE)

Don't define acronyms that won't be used often - use the full term instead.
## Active voice

You should write most of your documentation in the [active voice](https://developers.google.com/tech-writing/one/active-voice), not in the passive voice. The following table shows how active voice can make your sentences clearer to read:

| Passive voice                       | Active Voice               |
| ----------------------------------- | -------------------------- |
| Avatars can be created by anyone.   | Anyone can create avatars. |
| Products are contained in listings. | Listings contain products. |

The active voice also helps you highlight the actor in your sentence. The passive voice sometimes omits the actor:

| Passive voice                                 | Who is the subject? | Active voice                                      |
| --------------------------------------------- | ------------------- | ------------------------------------------------- |
| Wait for your content to be uploaded.         | The SDK             | Wait for **the SDK** to upload your content.      |
| This option must be enabled in the inspector. | You (the reader)    | **You** must enable this option in the inspector. |

## Clear sentences

Use strong verbs and subjects to write [clearer sentences](https://developers.google.com/tech-writing/one/clear-sentences). Strong verbs improve the clarity of your sentences and engage your readers. Avoid weak, imprecise, or generic verbs. For example:

| Weak verb  | Weak verb                                    | Strong verb                              |
| ---------- | -------------------------------------------- | ---------------------------------------- |
| **Be**     | **Be** careful not to exceed...              | **Ensure** that you don't exceed...      |
| **Occur**  | The issue **occurs** when upgrading the SDK. | Upgrading the SDK **causes** this issue. |
| **Happen** | Avatar performance issues **happen** if...   | Avatars **reduce** performance if...     |

Forms of "be" (is, are, was, were... ) are sometimes the best choice. You don't need to always replace them - but take the time to consider alternatives.

Sentences that start with "There is" combine a weak subject ("There") with a weak verb ("is"). Improve your sentences by replacing "There is" with strong subjects and verbs:

| Sentence with there is / are                                             | Sentence with strong verb and subject                       |
| ------------------------------------------------------------------------ | ----------------------------------------------------------- |
| There is an auto-layout option that arranges your windows automatically. | The auto-layout option arranges your windows automatically. |
| There are many ways to detect the VRChat SDK.                            | You can detect the VRChat SDK in many ways.                 |
| There is no guarantee the master player will respond.                    | The master player may not respond.                          |

## Short sentences

[Short sentences](https://developers.google.com/tech-writing/one/short-sentences) are usually easier to read, understand, and maintain than long sentences.

- Focus each sentence on a single idea. If your sentence contains multiple thoughts, separate it into multiple sentences.
- When you use the conjunction "or" in a long sentence, consider turning it into a bulleted list.
- Replace extraneous phrases with concise words. For example, replace "at this point in time" with "now." 

## Lists and tables

[Lists and tables](https://developers.google.com/tech-writing/one/lists-and-tables) can make your documentation easier to understand. 

- Introduce each list and table with a sentence. Tell readers what it represents.
- Capitalize the first letter of each item. If you use sentences, punctuate them.
- Items should "belong" with each other or be in a similar category.


The following list explains how to choose which type of list you should use:

| Type of list      | Example                                                            | Description                                                                                                                                                |
| ----------------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bulleted list** | <ul><li>Avatars</li><li>Worlds</li></ul>                           | Use bulleted lists for unordered items. Changing the order does not change the meaning of the list.                                                        |
| **Numbered list** | <ul><li>1. Download the SDK.</li><li>2. Install the SDK.</li></ul> | Use numbered lists for ordered items. Changing the order changes the meaning of the list.<br/>Start each item with an imperative verb, such as "download." |


:::warning

Markdown syntax does not support line breaks or lists inside tables. You can use HTML tags like `<br/>`, `<ul>`, and `<li>` instead, but this usually affects the table's formatting.

:::

## Paragraphs

[Paragraphs](https://developers.google.com/tech-writing/one/paragraphs) help the reader by breaking up complex ideas into smaller topics. Here's how to structure good paragraphs:

- Write a great opening sentence. It should establish what the paragraph is about.
- Focus each paragraph on a single topic. Move or remove sentences that don't relate to the current topic.
- Avoid very long paragraphs. Walls of text intimidate readers.
- Avoid very short paragraphs. Combine them or turn them into lists.

## Audience

When you write documentation, consider who your [audience](https://developers.google.com/tech-writing/one/audience) is and what you want them to learn. Here are some assumptions you can almost always make:

- Your audience has access to VRChat on Steam, Oculus, Pico, Android, or iOS. 
- Your audience knows about basic VRChat concepts such as players, avatars, worlds, friends, and virtual reality.
- Your audience speaks English but might not be native speakers.
- Your audience knows how to create a VRChat world or avatar Unity project with the Creator Companion.

VRChat's Creation documentation has a wide audience. Some pages are meant for beginners who need introductory guides. Other pages are for experts who are already familiar with the basics. If your audience already knows something, you don't need to repeat it. For example: 

- The audience of [Getting Started](/sdk/) wants to create something with the SDK.
	- You want to teach them how to use the Creator Companion to create a simple Unity project with the VRChat SDK.
	- You don't need to explain what VRChat is.
- The audience of [Avatar Scaling](/avatars/avatar-scaling) knows how to use the Avatars SDK.
	- You want to give them a detailed explanation about the limitations of avatar scaling.
	- You don't need to explain how to get started with the VRChat Avatars SDK.


---

## ドキュメント: syntax.md

```metadata
階層: /contribute/syntax.md
ディレクトリ: contribute
ファイル名: syntax.md
拡張子: .md
サイズ: 8.15 KB
最終更新: 2025-06-05T03:07:52.738Z
```

# Syntax guide

import BrowserWindow from '@site/src/components/BrowserWindow';
import APITable from '@site/src/components/APITable';

Most documentation pages consist of text. However, you can use additional syntax to make your pages easier to read.

## Markdown

Your pages are written in [Markdown](https://en.wikipedia.org/wiki/Markdown). Markdown documents are easy to edit and preview in any text editor. Here is a very simple example of a documentation page:

```md
# Example page

This is an example page with a title and some text.
```

The VRChat Creation documentation is built with [Docusaurus](https://docusaurus.io/). Docusaurus turns your Markdown file into a web page:

```mdx-code-block
<BrowserWindow>
```

# Example page

This is an example page with a title and some text.

```mdx-code-block
</BrowserWindow>
```

Docusaurus uses [MDX](https://mdxjs.com/), an extension of Markdown. You can use Docusaurus and MDX to enhance your documentation in various ways.

:::tip

You can learn how any page was created by clicking "Edit this page" at the bottom of any page.

:::

## Preview your changes

If you want to submit complex changes to VRChat's documentation, you should preview them on your computer. This allows you to see how your changes will look on the website.

To preview your changes, follow the [instructions on GitHub](https://github.com/vrchat-community/creator-docs?tab=readme-ov-file#local-development):
1. [Create your fork](https://github.com/vrchat-community/creator-docs/fork) of the [creator documentation repository](https://github.com/vrchat-community/creator-docs) on GitHub.
2. Clone your fork with [Git](https://www.git-scm.com/).
3. Install Docusaurus with [npm](https://www.npmjs.com/) by running `npm install` in the `Docs/` folder. 
4. Start Docusaurus with [npm](https://www.npmjs.com/) by running `npm run start` in the `Docs/` folder. 

## Front matter

[Front matter](https://docusaurus.io/docs/markdown-features#front-matter) is optional data about your markdown file. You can use it to change how Docusaurus presents your page in the documentation.

You can add front matter at the top of the file, enclosed by three dashes (`---`). The content is parsed as [YAML](https://yaml.org/spec/1.2.2/).

```
---
unlisted: true
---
```

The following table shows the most useful frontmatter fields. You can find a [full list in Docusaurus's documentation](https://docusaurus.io/docs/api/plugins/@docusaurus/plugin-content-docs#markdown-front-matter). 

```mdx-code-block
<APITable>
```

| Name                    | Type      | Default value         | Description                                                                                                | Recommendation                                                       |
| ----------------------- | --------- | --------------------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `sidebar_label`         | `string`  | Markdown title (`#`)  | The title of your page in the sidebar.                                                                     | Consider improving your page title instead of using `sidebar_label`. |
| `sidebar_position`      | `number`  | Alphabetical ordering | The position of your page in the sidebar.                                                                  | Only use `sidebar_position` when the sidebar order is important.     |
| `slug`                  | `string`  | Markdown file name    | The URL of your document.                                                                                  | Consider using an appropriate file name instead of using `slug`.     |
| `toc_min_heading_level` | `number`  | `2` (`##`)            | The minimum heading level shown in the table of contents. Must not be higher than `toc_max_heading_level`. | Consider creating multiple pages instead.                            |
| `toc_max_heading_level` | `number`  | 3 (`###`)             | The max heading level shown in the table of contents. Must be between `2` and `6`.                         | Consider creating multiple pages instead.                            |
| `unlisted`              | `boolean` | `false`               | Hides the page from the sidebar after being published on `creators.vrchat.com`.                            | Do not unlist pages that readers may find important.                 |
```mdx-code-block
</APITable>
```

- Do not use the [`title`](https://docusaurus.io/docs/api/plugins/@docusaurus/plugin-content-docs#unlisted) property. Use a Markdown title (`#`) instead.
- Do not use the [`last_update`](https://docusaurus.io/docs/api/plugins/@docusaurus/plugin-content-docs#last_update) property. It is automatically calculated.

## Admonitions

You can use admonitions to highlight short, important information. Admonitions stand out from the rest of our text.

- Don't overuse admonitions.
- Don't override admonition titles.


```
:::tip

Use this for important recommendations or shortcuts.

:::

:::info

Use this for important limitations or context.

:::

:::warning

Use this important warnings or potential errors.

:::

:::danger

Use this for actions that can lead to irreversible damage. 

:::
```

```mdx-code-block
<BrowserWindow>
```
:::tip

Use this for important recommendations or shortcuts.

:::

:::info

Use this for important limitations or context.

:::

:::warning

Use this for potential errors or how to avoid them.

:::

:::danger

Use this for actions that can lead to irreversible damage. 

:::
```mdx-code-block
</BrowserWindow>
```

## Code blocks

To include UdonSharp code on your page, use three backticks to create a [code block](https://docusaurus.io/docs/markdown-features/code-blocks). This is especially helpful for examples.

````md
```
// This is an example code block.
Debug.Log("Hello, world!");
```
````

```mdx-code-block
<BrowserWindow>
```

```
// This is an example code block.
Debug.Log("Hello, world!");
```

```mdx-code-block
</BrowserWindow>
```

You can make code blocks easier to read by enabling additional options:

````md
```csharp showLineNumbers title="Assets/Example.cs"
// This is an example code block.
Debug.Log("Hello, world!");
```
````

```mdx-code-block
<BrowserWindow>
```
```csharp showLineNumbers title="Assets/Example.cs"
// This is an example code block.
Debug.Log("Hello, world!");
```
```mdx-code-block
</BrowserWindow>
```

- Enable C# [syntax highlighting](https://docusaurus.io/docs/markdown-features/code-blocks#syntax-highlighting) with `csharp` or `cs`.
- Enable [line numbering](https://docusaurus.io/docs/markdown-features/code-blocks#line-numbering) with `showLineNumbers`,
- Show a [file name](https://docusaurus.io/docs/markdown-features/code-blocks#code-title) with `title=""`, if appropriate.

## Code Tabs

You can show an Udon Graph screenshot and UdonSharp code side by side. Readers can choose which language they prefer, and all code tab components synchronize the reader's preference.

Here's an example of how to import and use `Tabs` and `TabItem`:

````md
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![Three Udon Graph nodes that gets all players.](/img/worlds/graphgetplayers.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs showLineNumbers
var players = new VRCPlayerApi[VRCPlayerApi.GetPlayerCount()];  
VRCPlayerApi.GetPlayers(players);
```

</TabItem>
</Tabs>
````

```mdx-code-block
<BrowserWindow>
```

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs groupId="udon-compiler-language">
<TabItem value="graph" label="Udon Graph">

![A screenshot of the Udon Graph.](/img/worlds/graphgetplayers.png)

</TabItem>
<TabItem value="cs" label="UdonSharp">

```cs showLineNumbers
var players = new VRCPlayerApi[VRCPlayerApi.GetPlayerCount()];  
VRCPlayerApi.GetPlayers(players);
```

</TabItem>
</Tabs>

```mdx-code-block
</BrowserWindow>
```


---

# ドキュメント終了
