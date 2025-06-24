---
title: "Exploration of Digital World: My First Blog"
published: 2025-06-20T13:34:57+08:00
summary: Building a Hugo static blog from scratch
cover:
  image: /第一篇博客封面.png
tags:
  - 过程
categories: 学习
draft: false
lang:
---

## Background Introduction:
---
It's a long story,on June 11th, as my expected, I was informed that I didn't pass the interview part of teaching certificate examination. After a summary review, I decided to do English writing practice on certain platforms. I chose "langcorrect" among many suggestions giving by DeepSeek. 

On the evening of June 12th, I published my first English writing exercise *summary of half past  year*. The next morning, I received one feedback from an Austrian English native speaker, who fixed my mis-expressions and even the punctuation, illustrating natural expressions for my reference.

On the other hand, considering my career aspiration, I'd like to explore other options besides roles in international trade or teaching. Then I asked DeepSeek for more information about "technical  writer", a position I had heard about before. During a conversation, I was exposed to new concepts like "Git", "Markdown" and "VS Code".

After practicing basic Markdown grammar on StackEdit (a web-based editor), I moved to Obsidian, which unlocks truly immersive writing for personal blog. That sparked my motivation to finally launch my personal blog. According to DeepSeek, I chose Hugo, which is a suitable option for beginners. 
## Preparation:
---
- Git and your Github account
- Hugo
- VS Code
- Your domain 
- Markdown editor (Obsidian or others)
- Cloudflare account
## Process：
---
1. Install Hugo frame ( the version I installed was extended 0.147.8 windows amd64), then put 'exe' into 'bin' folder. Set up Git and Hugo paths in Environment Variables. Verify their runtime environments.

2. Create a Hugo project(your blog) using Git command via Powershell. You can check if your blog folder has been added in 'File Explorer'. 

3. Install 'PaperMod' theme. We can use Git command via Powershell to clone that theme from Github.

4. Use VS Code edit your 'hugo.yml' file. 

5. Run 'hugo server' command in VS Code terminal and then preview your blog site via the localhost URL (such as http://localhost:1313).

6. Add your first 'blog.md' file in 'content' folder. You can add cover and embed video in your blog by using Hugo shortcodes.

7.  Create a new repository on Github for your blog folders and comments. Here you may     have difficulties in pushing 'PaperMod' folder. So please pay attention that your 'posts'    folder and 'hugo_build.lock' file, you don't need to push these two into your repository,         so you're expected to create a '.gitignore' file in your blog folder. And make sure that      'posts' and 'hugo_build.lock' are contained in '.igtignore'. 

    Before you push blog folder (we call it as 'my_blog') into Github repository, you have to set up a new folder (We call it as blog_copy) and copy your whole blog folders in it, which makes it convenient for follow-up management. Then delete 'my_blog/Git' and 'my_blog/themes/PaperMod/.git'. 
    
    After this you can push 'my_blog' folder into Github repository via VS Code. Here the blog's comment system I use is 'giscus'. You should install 'giscus' on Github firstly. And then open 'discussion' permission and choose settings for your blog repository. After this you can verify functionality via the localhost URL (such as http://localhost:1313).                                                
8. Use Cloudflare deploy your Hugo blog project. Sign in tour Cloudflare and input your domain. Then choose 'Compute (workers) - Workers&Pages-Create-Import a repository (connecting your Github account)', select your blog repository. Pay attention to 'Environment variables, here I use HUGO_VERSION = 0.147.8 '. 

   After setting up your application click 'create and deploy'. It takes about 5 minutes that Cloudflare auto-provisions DNS and SSL for you, then you can access your blog site with   your domain or 'https://your domain'.
## Reference Tutorials：
---
1. [Hugo博客搭建教程以及配置调优](https://cloud.tencent.com/developer/article/2530969) 
2. {{< bilibili "BV1bu96YqEXd" >}}

