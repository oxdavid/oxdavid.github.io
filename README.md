## Git 分支开发流程介绍

`Feature Branch Workflow`

该工作流的核心理念为：`master`分支用于线上正式环境打`tag`部署，测试环境使用`test-master`分支，任何功能性开发或者缺陷开发应基础`master`分支 `checkout`出来一个新的`feature`分支或者`bugfix`分支，开发完成后，可以本地`merge`到 `test-master`分支上。当`test-master`分支测试没有问题，不能直接`merge`到`master`分支上，需要指定上线的`feature`分支或者`bugfix`分支发送`PR`到 `master` 分支，若`PR`没有冲突就直接合并,若有冲突谁负责的谁去解决冲突，解决完成后再本地在进行检查
 
### 1：工作流程

* 项目开始时从`master`分支`checkout`出来`test-master`分支，用于测试环境测试，后面 `master`和`test-master`分支就没关系了

* 从`master`分支`checkout`出来一个新的功能分支`feature-branch`进行新的功能开发

* 开发一段时间后，把功能分支提交推送到远程仓库，方便后面修改`bug`或者记录使用(当上线正式环境后可删除该功能分支)

* 开发完成后，提交到对应的远程并在本地进行`merge`到`test-master`分支上

* 发布`test-master`分支到测试环境进行测试

* 如测试有问题，测试人员在禅道等相关的项目管理工具上提出`bug`，开发人员可以在原来的功能分支上进行修改，修复成功后在合并到`test-master`分支，在进行测试

* 测试通过后，在`gitlab`后台管理界面从从需要上线的`feature`分支向`master`发送一个`PR`请求进行合并代码，若`PR`没有冲突就直接合并,若有冲突谁负责的谁去解决冲突，解决完成后再本地在进行检查

* 基于`master`打`tag`进行线上版本发布

* 若线上`tag`版本出现`bug` ，基于该`tag`的版本对应的`master`分支新建`bugfix`分支进行修复，紧急修复完成后把该`bugfix`分支合并到`test-master`分支上进行测试，如果没有问题，再把该`bugfix`分支`PR`到`master`进行合并，然后打`tag`进行发布版本

* 若`feature`为未计划上线的功能，不需要发送`PR`到`master`分支


### 2：分支规范

* feature分支

    采用`^(feature|bugfix|hotfix)\/\d+-(\w)$`命名

    * `feature`: 新功能，理论上是相互独立的
    * `bugfix`: 针对 master 分支的 bug进行修改
    * `hotfix`: 针对特定 tag 版本的 bug 进行修改
    * `\d`: 需求ID, 或者禅道上的 bug ID，若无该 ID ，可使用当前年月日代替 20190101
    * `\w`: 具体的分支名称

    示例：`feature/123-用户登录`，`bugfix/456-密码错误`，`hotfix/789-邮件发送`


* tag分支

    采用`tag-^\d{4}\.\d{1,2}\.\d{1,2}(\.\d+)?$`,---> 年.月.日.次数

    * `\d{4}` : 发布的年时间
    * `\d{1,2}`：发布的月时间
    * `\d{1,2}`：发布的日时间
    * `\d+`：当天的发布次数（可以不需要）

    示例：`2018.12.24.1`

### 3：分支管理

在向`master`发送一个`PR`请求进行合并代码，若当前`feature`或者`bug`分支不在需要时可以**勾选上删除源分支**

![pr_delete_branch](../说明图片/pr_delete_branch.png)

当项目开发一段时间后，会有好多功能分支或者缺陷分支，这时候可以利用 `gitlab` 自带的功能，删除已经合并的分支

![gitlab_delete_merge_branch](https://raw.githubusercontent.com/jjeejj/Pic_Save_Repo/master/gitlab_delete_merge_branch.png)
