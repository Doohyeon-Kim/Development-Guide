

## git flow
1. git branch는 main, release, development, feature로 나눈다.
    - main: 배포 branch
    - release: 배포 전에 통합 QA를 하기 위한 branch
    - develop: 개발 branch
    - feature: local repository에서 작업하며, 기능 구현을 완료하면 PR하여 develop에 merge하고 필요 없어진 feature branch는 삭제한다.
    - hotfix: 배포된 main에서 핫픽스가 필요한 경우 main으로부터 분기한다.
2. main은 매니저 허가 없이 절대 건들지 않는다.
3. merge conflict 발생 시 매니저 또는 팀에 공유하고 해결 방법 논의

<br>

## git commit template

```bash
# <type>: <subject>
##### Maximum length of subject is 50 ############## -> |
# content
######## Maximum length of content is 72 ########### -> |
# issue track number (#number)
# --- COMMIT END ---
# <type> list
#   feature : New feature
#   fix     : Fix bug
#   refactor: Refactor code
#   style   : Change style of code(white space, semicolon etc)
#   test    : Add/Change/Delete test case
#   docs    : Add/Change/Delete document
#   build   : Change in build script
#   ci      : Change in ci script
#   chore   : etc
# ------------------
#     Capitalize the subject line
#     Use imperitive mood in subject
#     Do not use period at the end of subject
#     Seperate subject and content with a blank line
#     Use the body explain "what" and "why" vs "how"
#     Use "-" when content contains multiline
# ------------------
```
  
```bash
# <type>: <subject>
##### 제목의 최대 길이는 50(영문), 한글(25) ############## -> |
# content
######## 내용의 최대 길이는 72(영문), 한글(36) ########### -> |
# issue track number (#number)
# --- COMMIT END ---
# <type> list
#   feature : 새로운 기능 구현
#   fix     : 버그 수정
#   refactor: 코드 리팩토링
#   style   : 코드 스타일 변경(세미콜론, 들여쓰기 등)
#   test    : 테스트 케이스 추가/변경/삭제
#   docs    : 문서 추가/변경/삭제
#   build   : 빌드 스크립트 변경
#   ci      : CI 스크립트 변경
#   chore   : 기타
# ------------------
#     제목은 대문자로 시작 한다.
#     제목은 명령형으로 작성한다.
#     제목 끝에 마침표를 사용하지 않는다.
#     제목과 내용은 공백으로 구분한다.
#     본문에 "어떻게"보다는 "무엇"과 "왜"를 설명한다.
#     내용에 여러줄이 포함된 경우 "-"를 사용한다.
# ------------------
```
  
<br>

## How to use git commit template

### Write .gitmessage file at $HOME.

``` bash
vim ~/.gitmessage
```

Copy & paste the template above into the .gimessage file.

<br>

### add .gitmessage file to commit.template

global
``` bash
git config --global commit.template ~/.gitmessage
```

local
``` bash
git config commit.template ./path/to/.gitmessage
```

<br><br>

## PR template

### Detailed description pull request template

``` bash
# Description

Please include a summary of the changes and the related issue. Please also include relevant motivation and context. List any dependencies that are required for this change.

Fixes # (issue)

## Type of change

Please delete options that are not relevant.

- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] This change requires a documentation update

# How Has This Been Tested?

Please describe the tests that you ran to verify your changes. Provide instructions so we can reproduce. Please also list any relevant details for your test configuration

- [ ] Test A
- [ ] Test B

**Test Configuration**:
* Firmware version:
* Hardware:
* Toolchain:
* SDK:

# Checklist:

- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] Any dependent changes have been merged and published in downstream modules
```

<br>

### Simple pull request template

```bash
- Summary
 
- What I did before request PR review.
 
- What I want after this PR review.

# Checklist:
- [ ] I have performed a self-review of my code
- [ ] My changes generate no new warnings
- [ ] New and existing unit tests pass locally with my changes
- [ ] I Followed the convention

```
