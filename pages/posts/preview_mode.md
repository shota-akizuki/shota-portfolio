---
title: Next.js + microCMSでPreview Modeを実装する
date: 2021/3/26
description: View examples of all possible Markdown options.
tag: web development
author: You
---

# Next.js + microCMS で Preview Mode を実装する

```
export default async (req, res) => {

  //次のパラメーターを確認。
  if (!req.query.id) {
    return res.status(401).json({ message: 'Invalid token' });
  }
  // 提供された `slug` が存在しているかどうか確認するため、ヘッドレスCMSをフェッチ。
  const post = await fetch(
    `https://shota-akizuki.microcms.io/api/v1/blog/${req.query.id}?draftKey=${req.query.draftKey}`,

    { headers: { 'X-API-KEY': process.env.API_KEY || '' } }
  )
    .then((res) => res.json())
    .catch((error) => null);



  // slugが存在しない場合、プレビューモードを有効にしないようにする、
  if (!post) {
    return res.status(401).json({ message: 'Invalid slug' });
  }
  // Cookiesを設定し、プレビューモードを有効にする
  // プレビューデータを格納
  res.setPreviewData({
    id: req.query.id,
    draftKey: req.query.draftKey
  });
  // 詳細ページへリダイレクト

  res.writeHead(307, { Location: `/blog/${req.query.id}` });
  res.end('Preview mode enabled');
};

```

## 参考にした記事

- [Next.js](https://nextjs-ja-translation-docs.vercel.app/docs/advanced-features/preview-mode)
- [microCMS](https://blog.microcms.io/nextjs-preview-mode/)
- [Qiita](https://qiita.com/itomise/items/fd7c9977aee6469378d0)
