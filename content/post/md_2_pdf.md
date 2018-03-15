---
title: "pandoc转换md"
date: 2018-03-02T13:06:01+08:00
draft: false
categories: ['杂项']
tags: ['markdown','pdf']
---

# markdown 如何转为pdf打印。  
## 解决方案：  
- 直接使用pandoc这个haskell应用

## 具体操作：  
1. 由于已经安装了haskell。直接使用cabal(相当于python的pip)安装编译。编译pandoc时间蛮久的，耐心等待吧    
  ```
  cabal update && cabal install pandoc
  ```
  注意点：这里可以使用清华的源来代替haskell的源 [替换方法](https://mirror.tuna.tsinghua.edu.cn/help/hackage/)
2. 安装tex发行版（可以理解为编译器吧）。一般linux安装[TeXLive](https://www.tug.org/texlive/),这个比较全一点，但是安装包很大，两三个G 。公司Mac下安装了MikTex。
3. 查看自己系统可用的中文字体：  
```
fc-list | grep lang=zh
```   
mac 下直接查看字符集 book.app
4. 下载tzengyuxio写好的latex模板 。[github](https://github.com/tzengyuxio/pages/tree/gh-pages/pandoc)  
注意latex模板中的字体要改为系统中的字体。   
```
\setCJKmain{这里修改要设置的字体}
```    
注意点：由于这个模板比较老。需要在`\begin{}`前添加一段    
```
\providecommand{\tightlist}{%
  \setlength{\itemsep}{0pt}\setlength{\parskip}{0pt}}
```
5. 直接使用pandoc套用模板进行转换    
```
pandoc --pdf-engine=xelatex -V CJKmainfont="STSong" xxx.md -o xxx.pdf --template=yourtemplate.latex
```

## 总结

- latex可以理解为为tex语言的高级封装。
- pandoc 转换中文时需要制定pdf引擎为xelatex


  
  
