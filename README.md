- 启动hexo本地服务
  - 执行`hexo s`
  - 在网页上输入 http://localhost:4000 可以查看服务是否启动成功（一般用来本地测试预览）。
- 创建博文
  - 执行`hexo n [layout] <title>`
  - 新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。
- 清除缓存
  - 执行`hexo clean`
  - 清除缓存文件和已生成的静态文件
  - 在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。
- 生成静态文件
  - 执行`hexo g`
- 上传部署博客
  - 执行`hexo d`、
  - 部署前要先通过`hexo g`生成静态文件