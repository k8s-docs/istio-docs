| Site                 | Status                                                                                                                                                                 |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| istio.io             | [![Netlify Status](https://api.netlify.com/api/v1/badges/c98435af-5464-4ac3-93c2-9c98faeec9b6/deploy-status)](https://app.netlify.com/sites/istio/deploys)             |
| preliminary.istio.io | [![Netlify Status](https://api.netlify.com/api/v1/badges/a1cfd435-23d5-4a43-ac6d-8ec9230d9eb3/deploy-status)](https://app.netlify.com/sites/preliminary-istio/deploys) |
| archive.istio.io     | [![Netlify Status](https://api.netlify.com/api/v1/badges/f8c3eecb-3c5c-48d9-b952-54c7ed0ece8f/deploy-status)](https://app.netlify.com/sites/archive-istio/deploys)     |

## istio.io

This repository contains the source code for the [istio.io](https://istio.io),[preliminary.istio.io](https://preliminary.istio.io), and [archive.istio.io](https://archive.istio.io) sites.

Please see the main Istio [README](https://github.com/istio/istio/blob/master/README.md) file to learn about the overall Istio project and how to get in touch with us.
To learn how you can contribute to any of the Istio components, please see the Istio [contribution guidelines](https://github.com/istio/community/blob/master/CONTRIBUTING.md).

- [istio.io](#istioio)
- [编辑和构建](#%e7%bc%96%e8%be%91%e5%92%8c%e6%9e%84%e5%bb%ba)
- [版本和发布](#%e7%89%88%e6%9c%ac%e5%92%8c%e5%8f%91%e5%b8%83)
  - [版本如何工作](#%e7%89%88%e6%9c%ac%e5%a6%82%e4%bd%95%e5%b7%a5%e4%bd%9c)
  - [立即发布内容](#%e7%ab%8b%e5%8d%b3%e5%8f%91%e5%b8%83%e5%86%85%e5%ae%b9)
  - [创建一个版本](#%e5%88%9b%e5%bb%ba%e4%b8%80%e4%b8%aa%e7%89%88%e6%9c%ac)
    - [当 Istio 源代码被分支](#%e5%bd%93-istio-%e6%ba%90%e4%bb%a3%e7%a0%81%e8%a2%ab%e5%88%86%e6%94%af)
    - [在发布之日起](#%e5%9c%a8%e5%8f%91%e5%b8%83%e4%b9%8b%e6%97%a5%e8%b5%b7)
      - [创建发布分支](#%e5%88%9b%e5%bb%ba%e5%8f%91%e5%b8%83%e5%88%86%e6%94%af)
    - [更新 istio.io](#%e6%9b%b4%e6%96%b0-istioio)
      - [更新 archive.istio.io](#%e6%9b%b4%e6%96%b0-archiveistioio)
      - [更新 preliminary.istio.io](#%e6%9b%b4%e6%96%b0-preliminaryistioio)
  - [创建一个补丁发布](#%e5%88%9b%e5%bb%ba%e4%b8%80%e4%b8%aa%e8%a1%a5%e4%b8%81%e5%8f%91%e5%b8%83)
- [多语言支持](#%e5%a4%9a%e8%af%ad%e8%a8%80%e6%94%af%e6%8c%81)
- [定期保养](#%e5%ae%9a%e6%9c%9f%e4%bf%9d%e5%85%bb)

## 编辑和构建

要了解如何编辑和建立这种回购的内容，请参阅[创建和编辑页面](https://preliminary.istio.io/about/contribute/creating-and-editing-pages/).

## 版本和发布

Istio 维护其公众网站的三个变化。

- [istio.io](https://istio.io) 是主要的网站，显示了产品的当前版本的文档。
- [archive.istio.io](https://archive.istio.io) 包含了产品的先前版本的文档的快照。这对于客户仍然使用这些旧版本是有用的。
- [preliminary.istio.io](https://preliminary.istio.io) 包含了产品的下一个版本的动态更新文档。

用户可以使用在每一页的右上角的齿轮菜单的网站的不同变化之间平凡导航。
所有这三个网站都托管在[Netlify](https://netlify.com).

### 版本如何工作

- 文档更改主要致力于 istio.io 的主分支。
  致力于这一分支的变化是在 preliminary.istio.io 自动反映。

- istio.io 的内容从最新的 release-XXX 分支跳转。
  所使用的特定部门由 istio.io [Netlify](https://netlify.com)项目的配置决定。

- archive.istio.io 的内容是从旧 release-XXX 分支服用。
  被包括在 archive.istio.io 该组分支由`TOBUILD`变量在此[脚本](https://github.com/istio/istio.io/blob/master/scripts/build_archive_site.sh)确定.

### 立即发布内容

Checking in updates to the master branch will automatically update preliminary.istio.io, and will only be reflected on istio.io the next time a release is created, which can be several weeks in the future.
If you'd like some changes to be immediately reflected on istio.io, you need to check your changes both to the master branch and to the current release branch (named release-XXX such as release-1.4).

This process can be taken care of automatically by our infrastructure.
If you submit a PR to the master branch and annotate the PR with the `actions/merge-to-release-branch` label, then as soon as your PR is merged into master, it will be merged into the current release branch.

### 创建一个版本

下面是步骤需要创建一个新的文件版本。
假设 Istio 的当前版本是 1.3，你希望引进 1.4 这已经在开发中。

#### 当 Istio 源代码被分支

文档回购拉从 Istio 源回购包含在发布的网站内容。
当源回购在制备是支化以便在一释放，需要在文档回购一些变化来跟踪此:

1. Switch to the **master** branch of the istio/istio.io repo and make sure everything is up to date.

1. Edit the file `Makefile.core.mk` and change the `SOURCE_BRANCH_NAME` variable to the
   name of the newly created source branches (in this case `release-1.4`).

1. Edit the file `data/args.yml` and set the `source_branch_name` field to the name of the newly created source
   branches (in this case `release-1.4`).

1. Run `make update_all` in order to retrieve the latest material from the source repositories.

1. Commit the previous edits to your local git repo and push your **master** branch to GitHub.

#### 在发布之日起

##### 创建发布分支

The day of a major Istio release, assuming you've previously done the steps from the above section, you need to:

1. Switch to the **master** branch of the istio/istio.io repo and make sure everything is up to date.

1. Edit the file `scripts/build_archive_site.sh` and add the new archive version
   (in this case `release-1.3`) to the `TOBUILD` variable.

1. Edit the file `data/versions.yml`. Set the `preliminary` field to the next Istio release
   (in this case `1.5`) and the `main` field to the current release (in this case `1.4`).

1. Commit the previous edits to your local git repo and push your **master** branch to GitHub.

1. Create a new release branch off of master, named as release-**major**.**minor** (in this case `release-1.4`). There is one
   such branch for every release.

1. Edit the file `data/args.yml`. Set the `preliminary` field to `false`
   and the the `doc_branch_name` field to the name of the release branch (in this case `release-1.4`).

1. Commit the previous edits to your local git repo and push your **release** branch to GitHub.

#### 更新 istio.io

1. 转到[Netlify](https://netlify.com)上的 istio.io 项目
2. 改变是从以前的版本的分支内置到新版本分支的分支，在这种情况下 release-1.4
3. 选择立即触发重建和重新部署的选项。
4. 一旦部署完成后，浏览 istio.io，并确保一切看起来不错。

##### 更新 archive.istio.io

1. Go to the [Google Custom Search Engine](https://cse.google.com) and do the following:

   - Download the archive.istio.io CSE context file from the Advanced tab.

   - Add a new FacetItem at the top of the file containing the previous release's version number. In
     this case, this would be "V1.3".

   - Upload the updated CSE context file to the site.

   - In the Setup section, add a new site that covers the previous release's archive directory. In this
     case, the site URL would be archive.istio.io/v1.3/\*. Set the label of this site to the name of the
     facet item created above (V1.3 in this case).

1. In the **previous release's** branch (in this case `release-1.3`), edit the file `data/args.yml`. Set the
   `archive` field to true and the `archive_date` field to the current date, and the `archive_search_refinement`
   to the previous release version (in this case `V1.3`).

1. In the **previous release's** branch (in this case `release-1.3`), edit the file `config.toml`. Set the
   `disableAliases` field to `false`.

1. Commit the previous edits to your local git repo and push the **previous release's** branch to GitHub.

1. In the **archive** branch, rebase the branch to have all changes from the current release. In this case,
   all changes from the `release-1.4` branch.

1. Commit the previous edits to your local git repo and push the **archive** branch to GitHub.

1. Wait a while (~20 minutes) and browse archive.istio.io to make sure everything looks good.

##### 更新 preliminary.istio.io

1. In the **master** branch, edit the file `data/args.yml`. Set the `version` and `full_version` fields to have the version
   of the next Istio release, and `previous_version` to be the version of the previous release. In this case, you would set the fields to
   "1.5", "1.5.0", and "1.4" respectively.

1. In the **master** branch, edit the file `data/args.yml`. Set the
   `source_branch_name` and `doc_branch_name` fields to `master`.

1. In the **master** branch, edit the file `Makefile.core.mk`. Set the variable `SOURCE_BRANCH_NAKE` to
   `master`.

1. Run `make update_all` in order to retrieve the latest material from the source repositories.

1. Commit the previous edits to your local git repo and push the **master** branch to GitHub.

1. Wait a while (~5 minutes) and browse preliminary.istio.io to make sure everything looks good.

### 创建一个补丁发布

Creating a new patch release involves modifying a few files:

1. Create the release note for the release by adding a markdown file in
   `content/en/news/<YEAR>/1.X.Y/index.md`, where 1.X.Y is the name of the release. This is where
   you describe the changes in the release.

1. Edit the `data/args.yml` file and change the `full_version` field to the name of the release.

1. Run `make update_ref_docs` to get the latest reference docs.

For the release note file, please look at existing files in the same location for example content and
layout.

## 多语言支持

该网站被翻译成多国语言。
真理的来源是英文内容，而其他语言的衍生等等往往滞后咯。
每个站点的语言都有自己的完全独立的内容目录和翻译表文件。
语言是用它们的国际两个字母的语言代码标识。
主要网站内容位于`content/<language code>` (e.g. `content/en`), 和转换表是在`i18n\<language code>.toml`一个 TOML 格式文件 (e.g. `i18n/en.toml`).

入门翻译很简单:

- 为你的语言创建`content/en`目录的完整副本。
  例如，如果你正在做一个法语翻译，复制`content/en` 到 `content/fr`。

- 更新所有在您的新内容目录指向的链接，内容目录，而不是到英文内容。
  For example, if you were doing a French translation you would change links such as `[a doc](/docs/a/b/c)` to `[a doc](/fr/docs/a/b/c)`.

- 删除所有`aliases`中的所有内容页面的前事指令。
  移动页面到新的位置时，别名使用，所以他们并不希望全新的内容。

- Create a copy of the `i18n/en.toml` file for your language.
  For example, you'd copy `i18n/en.toml` to `i18n/fr.toml` if you were doing a French translation.
  This file contains the text that is displayed by the site infrastructure for things like menus, and other standard material.

- Edit the file `config.toml` to list your new language.
  Search for the `[languages]` entry and just add a new entry.
  This tells the Hugo site generator to process your content.

- Edit the file `scripts/lint_site.sh` and search for `check_content`.
  Add another call to `check_content` for your new content directory.
  This ensures that the linting rules apply to your new content.

- 编辑文件`src/ts/lang.ts`，并添加新的语言。
  这将您的语言添加到切换按钮语言上可用 preliminary.istio.io，将使它所以你的语言将在语言选择菜单的支持。

- Get an Istio GitHub administrator to create a new maintainer team for your language. For Franch, this would be `WG - Docs Mintainers/French`.

- Edit the file `CODEOWNERS` and add entries for your language to give the new team you've created ownership over the translated content and translation table file.

You can then commit all of these changes and you can start translating the content and the translation file in a purely
incremental fashion. When you build the site, you'll find your content at `<url>/<language code>`. For example, once you've
checked everything in, you should be able to get to your content at `https://preliminary.istio.io/fr` if you were doing a
French translation.

Once your translation is complete and you're ready to publish it to the world, there are a few other changes you need to make:

- Edit the file `layouts/index.redir`. Search for `translated sites` and add a line for your language. This will cause
  users coming to the site for the first time to be automatically redirectded to the translated content suitable for them.
  For French, this would be:

      ```
      /  /fr   302  Language=fr
      ```

- Edit fhe file `layouts/partials/headers.html`. Search for `switch-lang` and you'll find the definitions for the language selection
  menu. Add a line for your new language.

And that's it.

## 定期保养

We have a number of checks in place to ensure a number of invariants are maintained in order to
help the site's overall quality. For example, we disallow checking in broken links and we do spell
checking. There are some things which are hard to systematically check through automation and instead
require a human to review on in a while to ensure everything's doing well.

It's a good idea to run through this list before every major release of the site:

- Ensure that references to the Istio repos on GitHub don't hardcode branch names. Search for any uses of `/release-1` or `/master`
  throughout all the markdown files and replace those with {{< source_branch_name >}} instead, which produces a version-appropriate
  branch name.

- Review the .spelling file for words that shouldn't be in there. Type names in particular tend to creep in here. Type names should
  not be in the dictionary and should instead be shown with `backticks`. Remove the entries from the dictionary and fix any spell
  checking errors that emerge.

- Ensure proper capitalization. Document titles need to be fully capitalized (e.g. "This is a Valid Title"),
  while section headings should use first letter capitalization only (e.g. "This is a valid heading").

- Ensure that preformatted text blocks that reference files from the Istio GitHub repos use the @@ syntax
  to produce links to the content. See [here](https://istio.io/about/contribute/creating-and-editing-pages/#links-to-github-files)
  for context.
