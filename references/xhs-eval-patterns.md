# XHS 通用提取模板

## 基础 Evaluate 模板

```js
() => {
  const pickText = (el, sels) => {
    for (const s of sels) {
      const v = el.querySelector?.(s)?.textContent?.trim();
      if (v) return v;
    }
    return '';
  };

  const num = (v) => {
    const m = String(v || '')
      .replace(/,/g, '')
      .match(/\d+(?:\.\d+)?/);
    return m ? Number(m[0]) : 0;
  };

  return [...document.querySelectorAll('.note-item, .comment-item, li, [data-item]')]
    .slice(0, 20)
    .map((el) => ({
      title: pickText(el, ['.title', '.note-title', 'h1', 'h2', 'h3']),
      hook: pickText(el, ['.desc', '.description', '.summary', '.intro']),
      angle: pickText(el, ['.tag', '.category', '.angle']),
      comments_signal: pickText(el, ['.comment', '.comments', '[data-comment]']),
      cta: pickText(el, ['.cta', '.action', '.footer']),
      likes: num(pickText(el, ['.like', '.likes', '[data-like]'])),
      tags: pickText(el, ['.tag-list', '.tags'])
    }))
    .filter(x => x.title || x.hook);
}
```

## 使用建议

- 先确认字段存在；缺失返回空字符串，避免脚本失败。
- 先做 20 条以内试跑，再扩大样本规模。
- 需复用时可按页面结构调整 selector。
