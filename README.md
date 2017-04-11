# tracerlee-hexo

Hexo源码

## TODO

- [ ] 使用Travis CI自动部署Hexo
- [ ] 添加评论功能
- [x] 统计
- [ ] 添加阅读数
- [ ] 添加剩余字数和阅读时间

## How to update

```bash
git pull origin source:source
git push origin source
```

## How to write

```bash
# Warning! Do not use Git Bash Client.
# 1. start server
hexo server
# 2. new a post
hexo new 'title'
# 3. deloy to github
hexo g -d
```

**参考：** http://tracer.xin/2016/04/03/hexo-tips/