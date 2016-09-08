.. _hg:

===========
hg 简明教程
===========

作者: 王然 kxxoling@gmail.com


关于 hg
----------------------

hg 即 Mercurial，是一个跨平台的分布式版本控制软件，主要由Python语言实现，
但也包含一个用C语言实现的二进制比较工具。其使用方式即原理与 git 相似，
如果你已经有 git 使用经验，可以访问 
`git 和 hg 面对面 <http://www.worldhello.net/2011/03/10/2370.html>`_


hg 基本流程
-----------------------

以 42web https://bitbucket.org/z42/z42 为例，演示 hg 开发流程。

1. 从远程仓库复制到本地
    https 协议从远程仓库获取代码::

        hg clone https://bitbucket.org/z42/z42

    或者也可以通过 ssh 协议::

        hg clone ssh://hg@bitbucket.org/z42/z42

#. 添加新创建的文件::

        hg add file_name

#. 修改本地代码并提交

    保存修改后，提交本地代码：::

        hg commit -m "change log"

    或者也可以简写为：::

        hg ci -m "change log"

    -m 表示关于本次提交的相关信息

    commit 时 hg 会自动 add 代码库中已修改的文件。


#. 提交到远程仓库前，首先检查远程代码状态

    可以使用 hg diff branches 查看不同分支间差异

    使用 hg fetch 命令从其它分支拉取代码并合并，如合并代码出现问题，
    需要手动合并后再提交。


#. 将代码提交到远程仓库::

    hg push

#. 发起 pull request

    在 https://bitbucket.org/你的用户名/项目名/pull-request/new 发起一个新的 pull request


解决冲突
---------------------

在 fetch 他人代码的时候，时常会遇到合并冲突的问题，因为有可能两人同时修改了同一个文件，这时需要先解决冲突（直接修改有冲突的文件），解决冲突后将其标注为已解决::

    hg resolve -m file.name


Tips
----------------------

* 向版本库添加/删除文件 `hg add/rm <path>`

* 移动版本库中的文件 `hg mv`

* 查找某段代码的责任人 `hg blame`

* 从现有代码初始化版本仓库 `hg init`

* 建立 hg 服务器 `hg serve`

* 更新代码库至最新提交 `hg update`

* 切换至分支 `hg update -r <branch>`

* 放弃所有修改，返回至上一个提交 `hg update -C`

* 搜索 `hg grep`

* 放弃某个文件的修改 `hg revert`


分支命名规则
------------

命名示例： \* bug/index\_page \* feature/founder\_page

分支的目的如果是 bug 修复以 ``bug/`` 作为开头；同理，新需求则以
``feature/`` 开头。这样能够明显地区分需求与 bug，并且以 ``/``
为分隔符能够得到 SourceTree 这样的图形化版本控制工具更好的支持。

分支名中不需要添加创建者，因为一个分支通常会有多个开发者（一个前端一个后端）同时使用。


内部 Hg 服务器
--------------

公司内部基于 `Kallithea <https://kallithea-scm.org/>`_ 开源系统搭建了一套 Hg web
服务，内网配置 DNS 后可访问 `hg.pe.vc <http://hg.pe.vc/>`_ 注册账号并 fork 所需要的项目。
使用流程如下：

1. `注册 <http://hg.pe.vc/_admin/register>`_ 并联系管理员激活

#. 登录并 `添加项目组 <http://hg.pe.vc/_admin/repo_groups/new>`_ 。

#. 前往 ``http://hg.pe.vc/<项目名>/fork`` 页面 fork 相应的项目。

其余操作和 GitHub、BitBucket 并无显著区别。

配置 Hg 记住密码功能
~~~~~~~~~~~~~~~~~~~~

通过 Hg HTTP 协议操作项目时，往往需要重复输入密码，不像 ssh 方式便捷，可以通过插件实现 Hg 记住密码功能：

.. code-block:: shell

    pip install mercurial_keyring

在 `~/.hgrc` 文件后写入：

.. code-block:: shell

    [extensions]
    mercurial_keyring =



扩展阅读
----------------------

`Hg－42 区漫游指南 <http://doc.42qu.com/tool/hg.html>`_

`git 和 hg 面对面 <http://www.worldhello.net/2011/03/10/2370.html>`_

`HgInit 中文版 <http://bucunzai.net/hginit/>`_
