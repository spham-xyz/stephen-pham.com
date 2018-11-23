---
title: "Various Webpack Loader Configurations"
date: 2018-11-22
tags: ["javascript"]
topics: ["webpack"]
---

One confusing aspect of webpack is recognizing the various webpack loader configurations. This is due in part to webpack maintaining backward compatibility with prior versions. Here are five different methods to specify loaders in the configuration file:

```
{ test: /\.scss$/, use: ['style-loader','css-loader','sass-loader'] }
{ test: /\.scss$/, use: [{loader:'style-loader'}, {loader:'css-loader'}, {loader:'sass-loader'}] }
{ test: /\.scss$/, loaders: ['style-loader','css-loader','sass-loader'] }
{ test: /\.scss$/, loaders: [ {loader:'style-loader'}, {loader:'css-loader'}, {loader:'sass-loader'}] }
{ test: /\.scss$/, loader: 'style-loader!css-loader!sass-loader' }
```

In webpack 1, it was possible to specify the loader name without the "-loader" suffix, e.g., 'style-loader' could be specified just as 'style'.  This is no longer allowed.