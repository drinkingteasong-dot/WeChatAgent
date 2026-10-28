# GitHub 上传 0.4.1+

仓库：`sourcepro5/WeChatAgent`。发布标题为 `WeChatAgent 0.4.1+`，Tag 为 `v0.4.1+`，按正式 Release 发布。

在公开版仓库根目录打开 PowerShell。构建完成后，可先预检再运行上传脚本：

```powershell
gh auth status
& '.\release\installer\Upload-0.4.1+.ps1' -DryRun
& '.\release\installer\Upload-0.4.1+.ps1'
```

未登录时先执行 `gh auth login`。上传脚本会验证源码文件集、全部资源哈希及 origin 地址，然后提交当前公开源码、推送 main 和新 Tag，最后上传 Release 附件。不强推、不覆盖现有 Tag 或 Release；远端有未合并提交时停止，需要合并后重新构建校验。

也可以手动执行以下步骤。任一步失败后先处理错误，再继续：

```powershell
npm run privacy:check
node scripts/verify-plugin-release.mjs
git add -A
git commit -m 'Release WeChatAgent 0.4.1+'
git tag -a 'v0.4.1+' -m 'WeChatAgent 0.4.1+'
git push --atomic origin main 'v0.4.1+'
```

确认源码和 Tag 已推送后，创建 Release：

```powershell
gh release create 'v0.4.1+' `
  '.\release\installer\WeChatAgent-Setup-0.4.1+-x64.exe' `
  '.\release\installer\WeChatAgent-0.4.1+-source.zip' `
  '.\optional-extensions\packages\fish-notebook-1.0.0.zip' `
  '.\optional-extensions\packages\speech-recognition-1.2.0.zip' `
  '.\release\installer\蓝色大肥鱼-小助手.md' `
  '.\release\installer\SHA256SUMS.txt' `
  '.\release\installer\VERIFICATION.json' `
  --repo sourcepro5/WeChatAgent `
  --verify-tag --latest `
  --title 'WeChatAgent 0.4.1+' `
  --notes-file RELEASE_NOTES.md
```

安装器及本机构建目录不提交进 Git，只作为 Release 附件上传。源码包、可选插件与校验清单须来自同一次最终验证；若提交或合并后更改了源码，应重新构建并验证。
