> **Note**
>
> 這份文件的目標為讓開發人員快速了解如何開發 repository；
> 請依註解 (`<!-- ... -->`) 調整，其中有 **REQUIRED** 表示必須填，填完後刪除所有註解！

## Setup the Development Environment

<!-- REQUIRED
請條列相依於哪些套件、SDK、官方連結，有需要注意的安裝步驟請寫清楚
-->
### Dependency

1. [.NET 6.0.406 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)

<!-- REQUIRED
請條列相依於 Nexus repository 以及如何設定
-->
### Binary Repository Management(BRM)

Gorilla 採用的的 BRM 工具為 [Nexus] (OSS)，與此 repository 相關的設定如下：

#### Docker

與此 repository 相關的 docker 會在 [nexus.gorilla-technology.com] 的 12345 port，因此，在 Docker Desktop for Windows 中從 Settings -> Docker Engine 內修改相關設定，並加入以下內容：

```json
{
    "registry-mirrors" : [
        "https://nexus.gorilla-technology.com:12345"
    ]
}
```

點選 `Apply & Restart`。
</details>

執行 `docker info` 確認有套用剛剛的設定：

```shell
docker info
 ...
 Registry Mirrors:
  https://nexus.gorilla-technology.com:12345
 ...
```

透過 docker login AD 帳號確認可以正常登入：
```shell
docker login https://nexus.gorilla-technology.com:12345
```

#### Nuget

開發過程使用的 nuget 套件會放在 Nexus 的 csharp-ci-nuget 內，因此請：
```shell
dotnet nuget add source https://nexus.gorilla-technology.com/repository/csharp-ci-nuget/index.json --name GORILLA_NEXUS --username <user> --password <password>
```

## 成為一個開發者吧

<!-- REQUIRED
請敘述此 repository 的開發模式
-->
Gorilla FBB 的開發流程為：

<details>
    <summary><i><a href="https://nvie.com/posts/a-successful-git-branching-model/">Git Flow</a></i></summary>

<p align="center"><img src="./images/02-Code/git-flow.png" alt="drawing" width="720"/></p>

  * 結構
    * 兩種主要分支
      * **develop**
        * 暫時性的整合
      * **main**
        * 打tag
      * 三種暫時分支
        * **feature**
          * 開發
        * **release**
          * 發布前的測試
        * **hotfix**
          * 緊急修復 
  * 使用情境
    * **開發 feature**
      * 從 develop 開 feature branch
      * 把 feature branch merge 回 develop
      * 刪除 feature branch
    * **準備 release**
      * 從 develop 開 release branch
      * 修正 bug
        * 在 release branch 進行 commit
        * 把 release branch merge 到 develop
    * **打 tag**
      * 把 release branch merge 到 main
      * 在 main 上打 tag
      * 把 release branch merge 回 develop
      * 刪除 release branch
    * **進行 hotfix**
      * 從 main 開 hotfix branch
      * 把 hotfix branch merge 回 main
      * 刪除 hotfix branch

  * 優點
    * 各分支分工明確、能應付幾乎所有專案需求
          即使是特殊版也能很方便的處理
  * 缺點
    * 結構很複雜，開發人員需要花較多時間去完整理解
    * 開 branch 和進行 merge 比較有機會出錯

</details>

<details>
    <summary><i><a href="https://docs.github.com/en/get-started/quickstart/hello-world">GitHub Flow</a></i></summary>

<p align="center"><img src="./images/02-Code/github-flow.png" alt="drawing" width="720"/></p>

  * 結構
    * 主要分支
      * **main**
    * 暫時分支
      * **feature**
        * 開發
        * 修復問題
  * 使用情境
    * **開發 feature**
      * 從 main 開 feature branch
      * 把 feature branch merge 回 main
      * 刪除 feature branch
    * **修正 bug**
      * 從 main 開 feature branch
      * 把 feature branch merge 回 main
      * 刪除 feature branch
    * **打 tag**
      * 在 main 上打 tag
  * 優點
    * 線性開發，固定從 main 開 branch、再 merge 回 main
          開發者可以很快的上手
    * 理想狀態下， main 是隨時在可以打 tag 的狀態
  * 缺點
    * 主要分支只有 main 一條
          不利於同時進行"開發"和"release 前的測試"
          
</details>

<details>
    <summary><i><a href="https://docs.gitlab.com/ee/topics/gitlab_flow.html">GitLab Flow</a></i></summary>

  有兩種模式(持續發布、版本發布)
  * 持續發布
    <p align="center"><img src="./images/02-Code/gitlab-flow-1.png" alt="drawing" width="720"/></p>

    * 兩種主要分支
      * **main**
      * **pre-production** (非必要，視專案需求): 發布前的測試
      * **production**: 打 tag
    * 一種暫時分支
      * **feature**: 開發
    * 用 `git cherry-pick` 把需要的 commit 拿到 production，而不是用 merge
    * 使用情境
      * **開發 feature**
        * 從 main 開 feature branch
        * 把 feature branch merge 回 main
        * 刪除 feature branch
      * **準備 release**
        * 從 main 用 cherry-pick 拿需要的 commit 到 pre-production
      * **修正 bug**
        * 從 main 開 feature branch
        * 把 feature branch merge 回 main
        * 從 main 用 cherry-pick 拿需要的 commit 到 pre-production
      * **打 tag**
        * 從 pre-production 用 cherry-pick 拿修正的 commit 到 production
        * 在 production 上打 tag
        * 如果 pre-production 有做過改動，用 cherry-pick 拿修正的 commit 到 main

  * 版本發布
    <p align="center"><img src="./images/02-Code/gitlab-flow-2.png" alt="drawing" width="720"/></p>

    * 主要分支
      * **main**
    * 暫時分支
      * **feature**: 開發
      * **stable**: 打 tag
    * 一條 stable 會對應一個發布版本，所以可能同時存在多條 stable
    * 用 `git cherry-pick` 把需要的 commit 拿到 stable，而不是用 merge
    * 使用情境
      * **開發 feature**
        * 從 main 開 feature branch
        * 把 feature branch merge 回 main
        * 刪除 feature branch
      * **準備 release**
        * 從 main 開 stable branch
      * **修正 bug**
        * 從 main 開 feature branch
        * 把 feature branch merge 回 main
        * 從 main 用 cherry-pick 拿修正的 commit 到 stable
      * **打 tag**
        * 在 stable branch 上打 tag
        * 如果 stable branch 有做過改動，用 cherry-pick 拿修正的 commit 到 main
        * 刪除 stable branch

* 優點
  * 除了 main 這個主要分支分支外，還會有額外的發布分支
        所以"開發"和"release 前的測試"可以很方便的同時進行
* 缺點
  * 因為是用`git cherry-pick`的方式來抽取需要的改動
        所以有可能會有漏拿 commit 的情況
        (可以用`git merge --no-ff`來輔助)
        (github 在 PR merge 時預設就是`--no-ff`)

</details>

以下提供簡易的步驟化開發指南：

### 0. Setup the issue status
   
<!-- REQUIRED
設定 issue 狀態
-->
若為解決 github.com/gorilla-ai/fbb 上描述的 issue，請明確將 issue 加上 `Status: WIP` 的 label，避免多人重複修改相同問題。

### 1. Clone code

<!-- REQUIRED
如果有 git lfs 請描述使用方式
-->
```shell
git clone https://github.com/gorilla-ai/fbb.git
```

### 2. Create feature/issue branch

<!-- REQUIRED
依據開發流程簡易的分支使用方式
-->
我們使用 `main` 作為主要、穩定的分支，因此請先切換到一個新的分支：
```shell
git checkout -b feat/implement-add-user-api main
```

### 3. Setup the development environment

<!-- REQUIRED
舉凡開發前需要設定的，都在這裡敘述
-->
```shell
dotnet restore
```

### 4. Start application

<!-- REQUIRED
執行專案需要注意的事項
-->
可以使用 VS Code 開啟專案，按下 F5 ...

### 5. Create commit

#### 5-0. 確保編譯都通過

<!-- REQUIRED
請描述如何編譯
-->
```shell
dotnet build .
```

#### 5-1. 確保 unit test 與 e2e test 都通過

<!-- REQUIRED
請描述如何執行 UT 以及 e2e test
-->
```shell
dotnet test .
```

#### 5-2. 確保 lint 沒有回報錯誤

<!-- REQUIRED
請描述如何執行 lint 工具
-->
```shell
dotnet format
```

#### 5-3. 新增 commit message
<!-- REQUIRED
請描述 commit 風格
-->
以 [Conventional Commits] 作為制定 commit message 風格：
    <p align="center"><img src="https://github.com/gorilla-ai/github-lab-conventional-commits/blob/main/images/convertional-commit.drawio.png" alt="conventional commit"></p>

   <details>
   <summary>簡介</summary>
   
   - <b>type</b> - Commit 的類型；最普遍使用的為 [@commitlint/config-conventional] 定義的：
     | Type     | Description                                                                                                 |
     | -------- | ----------------------------------------------------------------------------------------------------------- |
     | build    | Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)         |
     | chore    | Other changes that don't modify src or test files                                                           |
     | ci       | Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs) |
     | docs     | Documentation only changes                                                                                  |
     | feat     | A new feature                                                                                               |
     | fix      | A bug fix                                                                                                   |
     | perf     | A code change that improves performance                                                                     |
     | refactor | A code change that neither fixes a bug nor adds a feature                                                   |
     | revert   | Reverts a previous commit                                                                                   |
     | style    | Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)      |
     | test     | Adding missing tests or correcting existing tests                                                           |
    - <i>(scope)!</i> - 可選的，Commit 影響的範圍，例如：`api`
    - <b>description</b> - 簡短敘述
    - <i>body</i> - 可選的，詳細敘述
    - <i>footer(s)</i> - 可選的，描述對應修正哪個問題，例如：`Fixes #13`；或著是重大變更，例如：`BREAKING CHANGE: use JavaScript features not available in Node 6.`

    詳細的務必閱讀 [@commitlint/config-conventional]
   </details>

#### 6. Push to github.com
<!-- REQUIRED
-->
```shell
git push -u origin feat/implement-add-user-api
```

#### Create a pull request
<!-- REQUIRED
請描述 PR 後需要注意的事項
-->
請回到 https://github.com/gorilla-ai/fbb/pulls 發起一個 Pull Request 合併到 `main` 分支，並且留意以下事項：
- [ ] 是否正確指定 Reviewer？
- [ ] 是否在 PR 的敘述中[提到](https://github.com/gorilla-ai/infra-2022/blob/main/docs/DevOps-02-Code-Collaborators.md#linking-a-pull-request-to-an-issue)解決哪些問題？
- [ ] 是否有正確的設定 PR Label？
- [ ] PR 觸發的 Action 是否都有通過？

[nexus.gorilla-technology.com]:https://nexus.gorilla-technology.com/
[Nexus]: https://help.sonatype.com/repomanager3
[Conventional Commits]: https://www.conventionalcommits.org/zh-hant/v1.0.0/