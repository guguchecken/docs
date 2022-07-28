![4475675968e34a45fe58315c5b691ec](.imgs/labels/4475675968e34a45fe58315c5b691ec.png)

## Labels

上图显示的type of PR，其中`API-change`，`BUG`，`Improvement`，`Documentation`，`Feature`，`Code Refactoring`都已经有了对应的labels。

需新建label `kind/test`，使其对应`Test and CI`

## Mergify文件

~~~yaml
queue_rules:
  - name: default
    conditions: []  # no extra conditions needed to get merged

pull_request_rules:
  - name: Automatic merge on approval
    conditions:
      - base=main
      - "#approved-reviews-by>=1"
      - "check-success=Lint Docs"
      - "check-success=UT Test on Ubuntu/x64"
      - "check-success=UT Test on Darwin/x86"
      - "check-success=UT Test on Linux/Arm"
      - "check-success=BVT Test on Ubuntu/x64"
      - "check-success=BVT Test on Linux/Arm"
      - "check-success=BVT Test on Darwin/x86"
      - "check-success=BVT RACE Test on CentOS/x64"
      - "check-success=SCA Test on Ubuntu/x64"
    actions:
      queue:
        name: default
        method: squash
  - name: label for Bug
    conditions: 
      - body~=(?m)- \[(?i)x\] BUG
    actions:
      label:
        add:
          - kind/bug
  - name: label for Feature
    conditions: 
      - body~=(?m)- \[(?i)x\] Feature
    actions:
      label:
        add:
          - kind/feature
  - name: label for Improvement
    conditions: 
      - body~=(?m)- \[(?i)x\] Improvement
    actions:
      label:
        add:
          - kind/enhancement
  - name: label for Documentation
    conditions: 
      - body~=(?m)- \[(?i)x\] Documentation
    actions:
      label:
        add:
          - kind/documentation
  - name: label for Test and CI
    conditions: 
      - body~=(?m)- \[(?i)x\] Test and CI
    actions:
      label:
        add:
          - kind/test
  - name: label for Code Refactoring
    conditions: 
      - body~=(?m)- \[(?i)x\] Code Refactoring
    actions:
      label:
        add:
          - kind/refactor
  - name: label for API-change
    conditions: 
      - body~=(?m)- \[(?i)x\] API-change
    actions:
      label:
        add:
          - kind/api-change
~~~

