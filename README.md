# test-actions

这是一篇 Github Actions 学习记录，同时拿这个仓库来测试 Actions 工作流、作业的用法。

不定时更新。

## 检查 Secrets 是否存在

```yaml
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Get short SHA
        run: echo "SHORT_SHA=${GITHUB_SHA::7}" >> $GITHUB_ENV
        # 获取 Secrets 长度，保存到 env 中。与上一个作业执行过的 `>> $GITHUB_ENV` 不冲突
      - name: Check Secrets
        run: echo "SECRETS_LENGTH=$(expr length "$SECRETS_TO_CHECK")" >> $GITHUB_ENV
        env:
          SECRETS_TO_CHECK: ${{ secrets.NOT_EXISTS }}
        # 输出结果
      - name: Print Value
        run: echo "$SECRETS_LENGTH, $SHORT_SHA"
        # 如果没有指定 Secrets，不执行
      - name: Test Skip
        if: ${{ env.SECRETS_LENGTH > 0 }}
        run: echo "not skiped"
```

运行结果: ([#7](https://github.com/MrXiaoM/test-actions/actions/runs/8005591696/job/21865382064))
+ Print Value
```
Run echo "$SECRETS_LENGTH, $SHORT_SHA"
0, 245d8e2
```
