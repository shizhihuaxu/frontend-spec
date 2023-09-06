1. workflow、only、rules 规则的区别

   - workflow 是顶级关键字，决定了整个 pipeline 是否执行
   - only/except 只能限制分支和 tag 是否被执行
   - rules 可以在 workflow/only/except/job 中使用，可以设置更多条件

2. default 关键字设置全局的关键字的值，例如：

   ```
   default:
     image: node:16 # 所有的job均默认使用此image，可以在各job覆盖此默认值
   ```

   
