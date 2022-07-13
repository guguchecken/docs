# Bot需求

PR部分主要需求如下：

- Merge进行自动化处理（主要）
- 针对不同的情况添加或者删除label



针对merge方面，使用mergify就能满足需求，创建.mergify.yml文件如下：

在PR的提交分支为main分支，并且approve的人数大于等于1，通过五个对应的CI检查之后就会将其加入到merge queue。等待合并，合并使用squash方式。

若要提高Merge的速度，则可开启speculative_checks和batch_size，但是现在的速度应该足够。关于speculative_checks和batch_size：

> For example, by settings `speculative_checks: 2` and `batch_size: 3`, Mergify will create two pull requests: a first one to check if the first three enqueued pull requests are mergeable, and a second one to check the three next enqueued pull requests.

#### .mergify.yml

~~~yaml
queue_rules:
  - name: default
  	# speculative_checks: 2
    # batch_size: 2
    conditions: []  # no extra conditions needed to get merged

pull_request_rules:
  - name: Automatic merge on approval
    conditions:
      - base=main
      - "#approved-reviews-by>=1"
      - check-success="SCA Test on CentOS/x64"
      - check-success="UT Test on CentOS/x64"
      - check-success="BVT Test on CentOS/x64"
      - check-success="BVT RACE Test on CentOS/x64"
      - check-success="Lint Docs"
    actions:
      queue:
        name: default
        method: squash

~~~



针对label的添加与删除需求，mergify并不能很好的满足需求，其针对label的操作，需要在conditions部分设置条件触发，才能进行相应的label的添加与删除。触发的条件有限，常用的比如size标签相关操作不支持