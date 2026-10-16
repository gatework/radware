# radware

[![CI](https://github.com/gatework/radware/actions/workflows/ci.yml/badge.svg)](https://github.com/gatework/radware/actions/workflows/ci.yml)
[![Gem Version](https://badge.fury.io/rb/radware.svg)](https://rubygems.org/gems/radware)

面向 Radware 设备自动化的 Ruby gem。目前为初始骨架，提供命名空间、版本常量与基础异常；设备连接、配置解析和命令操作尚未实现。

## 安装

要求 Ruby 3.3 或更高版本。

```sh
gem install radware
```

或在 Gemfile 中添加：

```ruby
gem "radware", "~> 0.1.0"
```

## 使用

```ruby
require "radware"

Radware::VERSION # => "0.1.0"
Radware::Error   # StandardError 的子类
```

## 开发

```sh
bundle install
bundle exec rake
bundle exec rake package
```

CI 覆盖 Ruby 3.3、3.4、4.0。打包步骤在临时 GEM_HOME 中安装并加载 gem，检查版本与加载路径。

## 发布

1. 更新 `lib/radware/version.rb` 与 `CHANGELOG.md`，提交并推送到 `main`。
2. 等待该提交的 CI 通过，创建并推送同版本标签，例如 `v0.1.1`。
3. 在 GitHub 发布该标签的 Release；`release.yml` 会重新运行完整 CI，下载通过验证的 gem，发布到 RubyGems，再将相同的 gem 与 `SHA256SUMS` 附加到 Release。

```sh
git tag -a v0.1.1 -m "Release 0.1.1"
git push origin v0.1.1
gh release create v0.1.1 --verify-tag --title v0.1.1 --notes-file CHANGELOG.md
```

发布使用 GitHub `release` environment 的 `RUBYGEMS_API_KEY` secret。API key 需要 `push_rubygem` 权限，并允许非交互式发布；不要提交凭据。也可配置 [RubyGems Trusted Publishing](https://guides.rubygems.org/trusted-publishing/)（owner `gatework`、repository `radware`、workflow `release.yml`、environment `release`），随后删除该 secret，工作流将使用 OIDC。

失败时优先重新运行失败的 job，以复用原 CI 产物。`workflow_dispatch` 仅接受版本标签，并要求 GitHub Release 已存在。已发布的相同版本只有校验和一致才可继续；不同内容必须提升版本号。

## 许可

MIT，见 [LICENSE](LICENSE)。本项目由 gatework 维护，与设备厂商无隶属关系。
