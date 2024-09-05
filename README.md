# Examples of GROWI plugin

This repository is a collection of example plugins for GROWI.

## growi-plugin-remark-youtube

This plugin enables to embed YouTube videos in GROWI pages by Remark.

### [client-entry.tsx](https://github.com/goofmint/growi-plugin-remark-youtube/blob/30fc775df7fc260e06dad0045a60bfb252f505b3/client-entry.tsx)

```tsx
const activate = (): void => {
  if (growiFacade == null || growiFacade.markdownRenderer == null) {
    return;
  }

  const { optionsGenerators } = growiFacade.markdownRenderer;

  optionsGenerators.customGenerateViewOptions = (...args) => {
    const options = optionsGenerators.generateViewOptions(...args);
    options.remarkPlugins.push(plugin as any); // Add the plugin
    return options;
  };
};
```
### [src/youtube.ts](https://github.com/goofmint/growi-plugin-remark-youtube/blob/30fc775df7fc260e06dad0045a60bfb252f505b3/src/youtube.ts)

Plugin for Remark.

```ts
export const plugin: Plugin = function() {
  return (tree) => {
    visit(tree, (node) => {
      const n = node as unknown as GrowiNode;
      try {
        if (n.type === 'leafGrowiPluginDirective' && n.name === 'youtube') {
          const id = Object.keys(n.attributes)[0];
          const { width, height } = n.attributes;
          n.type = 'html';
          n.value = `<div style="width: 100%; aspect-ratio: 16/9">
            <iframe
              width="${width || '560'}"
              height="${height || '315'}"
              src="https://www.youtube.com/embed/${id}"
              // :
            </iframe>
          </div>`;
        }
      }
      catch (e) {
        n.type = 'html';
        n.value = `<div style="color: red;">Error: ${(e as Error).message}</div>`;
      }
    });
  };
};
```

