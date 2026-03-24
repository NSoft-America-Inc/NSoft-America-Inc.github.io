---
title: "NSoft America 기술 블로그 기여 가이드: 환경 설정부터 첫 포스팅 배포까지 (Mac/Windows)"
date: 2026-03-23
weight: 20
draft: false
tags: ["guide", "hugo", "github-pages", "collaboration", "onboarding"]
categories: ["General"]
description: "NSoft America 기술 블로그를 위한 Mac/Windows 환경 설정 및 포스팅 절차 안내서입니다."
author: "NSoft America"
---

## Overview
NSoft America 기술 블로그의 새로운 필진이 되신 것을 환영합니다! 본 블로그는 정적 사이트 생성기인 **Hugo**와 **GitHub Pages**를 기반으로 운영되며, 모든 포스팅 과정은 Git 워크플로우를 따릅니다. 

이 가이드는 처음 블로그에 참여하는 분들이 자신의 PC에 환경을 설정하고, 첫 번째 글을 작성하여 전 세계 독자들에게 배포하기까지의 모든 과정을 단계별로 안내합니다. 본 포스트 하나로 Mac과 Windows 사용자 모두가 막힘없이 설정을 완료할 수 있도록 구성되었습니다.

---

## Background / Problem: 왜 가이드가 필요한가?

기술 블로그는 단순히 지식을 기록하는 곳을 넘어, 팀의 기술적 역량을 대외적으로 증명하고 내부적으로는 지식을 공유하는 중요한 자산입니다. 하지만 여러 개발자가 동시에 참여하는 블로그 환경에서는 다음과 같은 문제들이 발생하곤 합니다.

1.  **환경 일관성**: 서로 다른 OS와 버전의 Hugo를 사용할 때 발생하는 빌드 오류
2.  **컨벤션 충돌**: 파일명 규칙이나 Front Matter 설정 실수로 인한 레이아웃 깨짐
3.  **배포 프로세스 오류**: 잘못된 브랜치에 푸시하거나 `draft` 설정을 깜빡하여 글이 보이지 않는 문제

이러한 시행착오를 줄이고 오직 '콘텐츠 작성'에만 집중할 수 있도록, NSoft America의 표준 포스팅 가이드를 마련했습니다.

---

## Solution / Implementation: OS별 환경 설정 및 워크플로우

### 1. 사전 준비 (Core Prerequisites)
어떤 운영체제를 사용하든 아래 두 가지 요건은 반드시 충족되어야 합니다.

- **GitHub 계정**: `NSoft-America-Inc` 조직(Organization)의 멤버로 등록되어 있어야 레포지토리에 푸시가 가능합니다. (미등록 시 관리자에게 요청하세요.)
- **Git 설치 및 인증**: 터미널이나 커맨드라인에서 `git` 명령어를 사용할 수 있어야 하며, GitHub와 SSH 또는 토큰(Fine-grained PAT) 방식으로 인증이 완료되어 있어야 합니다.

---

### 2. Mac 환경 설정 가이드

Mac 사용자는 패키지 매니저인 `Homebrew`를 사용하여 매우 간편하게 환경을 구축할 수 있습니다.

#### **Step 2-1: Homebrew 설치**
터미널을 열고 아래 명령어를 입력하여 Homebrew가 설치되어 있는지 확인하세요. 없다면 설치 매뉴얼을 따릅니다.
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

#### **Step 2-2: Hugo 설치 (Extended 버전)**
본 블로그의 테마는 향상된 기능을 위해 Hugo Extended 버전을 필요로 하는 경우가 많습니다. Homebrew를 통해 최신 버전을 설치합니다.
```bash
brew install hugo

# 설치 확인 (Extended 문구가 포함되어 있는지 확인)
hugo version
```

#### **Step 2-3: 레포지토리 클론 및 테마 초기화**
블로그 소스 코드를 내 로컬 PC로 가져옵니다. 이때 테마 데이터가 담긴 **Submodule**을 반드시 함께 초기화해야 합니다.
```bash
git clone https://github.com/NSoft-America-Inc/NSoft-America-Inc.github.io.git
cd NSoft-America-Inc.github.io

# 서브모듈(테마 등) 초기화 및 업데이트 (매우 중요!)
git submodule update --init --recursive
```

---

### 3. Windows 환경 설정 가이드

Windows 사용자도 최근에는 강력한 패키지 매니저를 사용할 수 있어 설정이 매우 편리해졌습니다.

#### **Step 3-1: Hugo 설치 (Winget 또는 Chocolatey)**
**Winget(Windows 11 이상 권장)**을 사용하는 경우:
```powershell
winget install Hugo.Hugo.Extended
```
또는 **Chocolatey**를 사용하신다면:
```powershell
# 관리자 권한 터미널에서 실행
choco install hugo-extended
```

설치 후 반드시 **터미널을 재시작**해야 `hugo` 명령어가 인식됩니다.
```powershell
hugo version
```

#### **Step 3-2: Git 설치 및 클론**
[Git 공식 홈페이지](https://git-scm.com/download/win)에서 다운로드하여 설치하세요. 설치 중 "Git from the command line and also from 3rd-party software" 옵션을 반드시 선택해야 합니다.

이후 Mac과 동일하게 레포지토리를 클론합니다.
```powershell
git clone https://github.com/NSoft-America-Inc/NSoft-America-Inc.github.io.git
cd NSoft-America-Inc.github.io
git submodule update --init --recursive
```

---

### 4. 실전! 포스트 작성 및 배포 워크플로우

환경 설정이 끝났다면 이제 첫 글을 써볼 차례입니다. 이 과정은 운영체제와 상관없이 동일합니다.

#### **1단계: 새 포스트 파일 생성**
Hugo의 `new` 명령어를 사용하여 날짜와 제목이 포함된 파일을 생성합니다.
```bash
# 날짜와 영어 제목(kebab-case)을 권장합니다.
hugo new posts/2026-03-24-my-first-post.md
```

#### **2단계: Front Matter 작성의 정석**
생성된 파일(`content/posts/파일명.md`)을 열어보면 상단에 메타데이터 설정 영역인 **Front Matter**가 있습니다. 아래는 NSoft America 블로그의 표준 설정입니다.

```yaml
---
title: "포스트 제목 (한글 가능)"
date: 2026-03-24
draft: false          # ← ⚠️ 중요: 배포 시 반드시 false여야 함
tags: ["tech", "hugo"] # 소문자 사용 권장
categories: ["General"] # 허용된 카테고리 중 선택
description: "이 포스트를 한 줄로 요약해주세요. SEO와 SNS 공유 시 설명으로 쓰입니다."
author: "NSoft America"
---
```

**⚠️ 초보 필진이 가장 많이 하는 실수:**
1.  `draft: true` 상태에서 배포: 사이트에 글이 절대 나타나지 않습니다.
2.  `date` 형식 오류: 반드시 `YYYY-MM-DD` 형식을 유지하세요.
3.  `description` 누락: 미리보기 화면에서 글이 썰렁해 보일 수 있습니다.

#### **3단계: 콘텐츠 작성 규칙**
- **Heading 사용**: Hugo는 `#` (H1)을 제목으로 자동 생성합니다. 따라서 본문에서는 반드시 `##` (H2)부터 사용하세요.
- **이미지 삽입**: 이미지는 `static/images/` 폴더에 먼저 저장한 후, 본문에서 `![설명](/images/파일명.png)`와 같은 형식으로 참조합니다.
- **코드 블록**: 기술 블로그인 만큼 코드의 신택스 하이라이팅은 필수입니다. 반드시 언어를 명시하세요 (예: ```javascript).

#### **4단계: 로컬 서버에서 미리보기**
글을 다 썼다면 GitHub에 푸시하기 전, 내 컴퓨터에서 어떻게 보이는지 먼저 확인해야 합니다.
```bash
hugo server
```
명령어 실행 후 브라우저에서 `http://localhost:1313`에 접속하여 레이아웃이나 오타가 없는지 꼼꼼히 체크합니다.

#### **5단계: 대망의 배포 (Git Push)**
문제가 없다면 실제 서버에 반영할 차례입니다.
```bash
git add .
git commit -m "post: 가이드 포스트 추가"
git push origin main
```
`main` 브랜치에 푸시하면 GitHub Actions가 자동으로 빌드를 시작하며, 약 1~2분 뒤 실시간 사이트에 반영됩니다.

---

## 심화: 문제 해결 및 주의사항 (Troubleshooting)

작업 도중 막히는 부분이 있다면 아래 표와 해결 방법을 참고하세요.

### **⚠️ 필수 체크리스트 (Common Mistakes)**

| 오류 상황 | 원인 | 해결 방법 |
| :--- | :--- | :--- |
| 사이트에 포스트가 안 보임 | `draft: true` 설정 | `false`로 변경 후 다시 push |
| 폰트나 CSS 레이아웃이 깨짐 | `baseURL` 소문자 규칙 위반 | `hugo.toml`에서 소문자 URL 확인 |
| 제목(H1)이 두 번 렌더링됨 | 본문에 `#` 사용 | `##` 이하의 헤딩만 사용 |
| 이미지 엑박(Broken Image) | 경로 설정 오류 | `static/` 경로는 `/`로 시작하는 절대경로 사용 |

### **💡 유용한 팁**
- **Submodule 오류 발생 시**: 테마가 정상적으로 보이지 않는다면 `git submodule update --init --recursive` 명령어를 다시 실행해 보세요.
- **배포 상태 확인**: GitHub 레포지토리의 **Actions** 탭을 클릭하면 빌드가 성공했는지, 실패했다면 어디서 에러가 났는지 상세히 볼 수 있습니다.

---

## 심화: 콘텐츠 포맷팅 마스터하기 (Advanced Formatting)

단순한 텍스트 나열을 넘어, 포스트의 가독성을 극적으로 높여주는 Hugo와 PaperMod 테마의 고급 기능들입니다.

### 1. 코드 블록에서의 행 강조 (Line Highlighting)
특정 행을 강조하고 싶을 때 코드 펜스 옆에 `{hl_lines=[행번호]}`를 추가할 수 있습니다.
```bash {hl_lines=[2]}
# 이 부분은 일반 텍스트입니다.
brew install hugo # 이 행이 강조됩니다!
```

### 2. 그림(Figure) 숏코드 활용
단순한 마크다운 이미지 태그(`![]()`)보다 더 제어권이 많은 `figure` 숏코드를 추천합니다. 캡션(설명)과 크기 조절이 가능합니다.
```markdown
{{< figure src="/images/setup-complete.png" title="설명" caption="빌드 성공 화면 예시" align="center" >}}
```

### 3. 알림창(Note/Notice) 스타일
중요한 주의사항을 강조할 때 독자의 시선을 사로잡을 수 있는 형식을 활용하세요. (테마 설정에 따라 다를 수 있으나, 일반적으로 인용문 `>`을 활용한 인포 박스를 추천합니다.)

---

## FAQ: 궁금해할 만한 질문들 (Frequently Asked Questions)

### Q1. 이미지를 꼭 `static/images/`에 넣어야 하나요?
네, Hugo의 표준 규칙입니다. `static` 폴더 아래의 파일들은 빌드 시 루트(`/`) 경로로 복사되므로, 본문에서는 `/images/파일명.png`와 같이 간결한 경로로 접근할 수 있습니다.

### Q2. 기존 글을 나중에 수정하고 싶을 때는 어떻게 하나요?
해당 글의 마크다운 파일을 수정한 뒤, 다시 `git add`, `commit`, `push`를 진행하면 됩니다. GitHub Actions가 변경 사항을 감지하여 약 2분 내에 자동으로 사이트를 업데이트합니다.

### Q3. 여러 명이 동시에 같은 파일을 수정하면 어떻게 되나요?
Git의 충돌(Conflict)이 발생할 수 있습니다. 이를 방지하기 위해 작업을 시작하기 전 항상 `git pull origin main`으로 최신 상태를 유지하고, 본인만의 브랜치를 생성해서 작업한 뒤 PR을 보내는 방식이 가장 안전합니다.

### Q4. 글의 대표 이미지(OpenGraph/Thumbnail)를 설정하고 싶어요.
Front Matter에 `images: ["/images/thumbnail.png"]` 항목을 추가하세요. SNS 공유 시 이 이미지가 카드의 썸네일로 사용됩니다.

---

## Key Takeaways
- **준비**: GitHub 조직 멤버 권한과 Hugo Extended 버전 설치는 필수입니다.
- **작업**: `hugo new`로 파일을 만들고, `draft: false` 설정을 잊지 마세요.
- **검증**: 반드시 로컬(`hugo server`)에서 실물을 확인한 뒤 푸시하세요.
- **공유**: 여러분의 소중한 지식 공유가 NSoft America 기술 블로그를 풍성하게 만듭니다.

## References (optional)
- [Hugo 공식 문서 (가이드라인)](https://gohugo.io/documentation/)
- [PaperMod 테마 위키](https://github.com/adityatelange/hugo-PaperMod/wiki)
- [NSoft-America GitHub Actions 배포 현황](https://github.com/NSoft-America-Inc/NSoft-America-Inc.github.io/actions)

---

## 마치며
NSoft America 기술 블로그는 구성원 한 분 한 분의 참여로 완성됩니다. 이 가이드가 여러분의 첫 걸음에 도움이 되기를 바랍니다. 설정 과정에서 어려움이 있다면 언제든 팀 동료들에게 도움을 청하세요. 여러분의 멋진 포스팅을 기대하겠습니다!
