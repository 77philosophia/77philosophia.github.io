# 77philosophia.github.io
own blog

hexo new
hexo g
hexo s
hexo d




# 简历管理指南 📄

本仓库使用 [RenderCV](https://github.com/sinaatalay/rendercv) 管理中英文简历。

## 常用命令
在Blog根目录下：
source venv_cv/bin/activate

- **初始化新简历**: `rendercv new "Wang Xin"`
- **预览渲染 (本地)**: `rendercv render wangxin_cv.yaml`
- **查看所有选项**: `rendercv --help`

## 工作流 (Workflow)
1. 修改 `resume_src/wangxin_cv.yaml`。
2. 运行 `rendercv render resume_src/wangxin_cv.yaml` 进行本地核对。
3. `git push` 触发 GitHub Actions 自动部署。

## 常见问题
- 如果中文乱码：确保 YAML 中 `design -> font` 包含支持中文的字体。
- 如果部署失败：检查 GitHub Actions 标签页的错误日志。
