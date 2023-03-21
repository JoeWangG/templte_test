> **Warning**
> Template 包含四個文件：
> 1. README.md
> 1. CONTRIBUTING.md
> 1. .github/ISSUE_TEMPLATE/*.yml
> 1. .github/pull_request_template.md
> 
> 以及兩個 workflow：
> 1. .github/workflows/lfs-warning.yml: 確認 commit 是否有尺寸過大的檔案
> 1. .github/workflows/release-drafter.yml: 協助產生 release drafter
> 
> 請斟酌調整。

> **Note**
>
> 這份文件的目標為讓管理階層、協同開發人員、開發人員快速了解 repository 的內容、狀態，而不是正式的對外文件、安裝文件、技術文件；
> 請依註解 (`<!-- ... -->`) 調整，其中有 **REQUIRED** 表示必須填，填完後刪除所有註解！

<!-- REQUIRED
請填入可辨別的標題，若之後的敘述會使用縮寫，請在這邊先寫全名後再寫縮寫
-->
# Gorilla Foo Bar Baz (FBB)

<!-- REQUIRED
請參考 https://github.com/gorilla-ai/github-lab-badge，至少包含 CI 相關的 workflow 狀態
-->
![](https://github.com/gorilla-ai/github-lab-badge/actions/workflows/blank.yml/badge.svg?branch=main)
[![Teams](https://img.shields.io/badge/chat-on_Microsoft_Teams-6264A7?style=flat)](https://teams.microsoft.com/l/channel/19%3aTXAuaXSO2g8gKGGHZrKLpFWC559gXp4BMH0RTMWdGvU1%40thread.tacv2/%25E4%25B8%2580%25E8%2588%25AC?groupId=4eb178fe-292f-46b5-aff2-cc7119eb7447&tenantId=fb1fa134-722a-4aa2-bc62-2eea53bf6f9a)
[![mailto](https://img.shields.io/badge/mail-to_repo_owner-0078D4?style=flat)](mailto:foo@gorilla-technology.com)
![](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/stan1ey-shen/fae9f1ddd01e2d636d7f7e4417d0f275/raw/build-result-0-errors-n-warnings.json)
![](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/stan1ey-shen/fae9f1ddd01e2d636d7f7e4417d0f275/raw/tests-result-n-passed-n-skipped-n-failed.json)
![](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/stan1ey-shen/fae9f1ddd01e2d636d7f7e4417d0f275/raw/coverage-80-60.json)

<!-- REQUIRED
簡短敘述功能
-->
Gorilla FBB is a ...

## Features

<!-- 
條列式敘述功能
-->
- ...
- ...

## Installation

<!-- REQUIRED
目標對象為協同開發人員，因此可以是如何取得 binary、可以是敘述如何安裝
-->
Gorilla FBB 釋出的版本皆儲存至 [Nexus](https://nexus.gorilla-technology.com/)：
```shell
dotnet nuget add source https://nexus.gorilla-technology.com/repository/csharp-release-nuget/index.json --name GORILLA_NEXUS --username <user> --password <password>
dotnet add package Gorilla.FBB
```

也可以透過 `--version` 指定版本：
```shell
dotnet add package Gorilla.FBB --version 1.0.3
```
或 prerelease 版本：
```shell
dotnet add package Gorilla.FBB --version 1.0.3-pre+sha.cb26a7b
```

<!-- REQUIRED
1. 元件相關的 repository，請提供數個使用範例
2. 程式碼請用 code block (```) 指定程式語言：https://www.rubycoloredglasses.com/2013/04/languages-supported-by-github-flavored-markdown/
-->
### Example

```csharp
var fbb = new FBB();
fbb.SayHi();
```

<!-- REQUIRED
目標對象為開發人員，連結至外部檔案 docs/CONTRIBUTING.md
-->
## Contributing

請參閱 [CONTRIBUTING.md](./CONTRIBUTING.md)。

<!-- REQUIRED
任何需要快速取得的文件列表
-->
## Documentation

- [Technical Specification](./docs/Technical_Specification.md)
- [OpenAPI descriptions](./docs/openapi.yml)

<!--
目標對象為協同開發人員、開發人員，可以條列常遇到的問題
-->
## FAQ

### 為什麼無法透過 Nuget 安裝？
請確認 Nexus 有加入至開發環境的 nuget source ：
```shell
PS:> dotnet nuget list source
  ...
  3.  GORILLA_NEXUS [已啟用]
      https://nexus.gorilla-technology.com/repository/csharp-release-nuget/index.json
```
