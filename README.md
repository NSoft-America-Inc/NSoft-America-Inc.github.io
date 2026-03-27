# NSoft America Tech Blog

NSoft America 공식 기술 블로그 레포지토리입니다. 
본 사이트는 **Hugo (Static Site Generator)**와 **GitHub Pages**를 통해 운영되며, 모든 기술적 노하우와 팀의 지식을 공유하는 것을 목적으로 합니다.

- **Live URL**: [https://nsoft-america-inc.github.io/](https://nsoft-america-inc.github.io/)

---

## 🤖 AI 기반 빠른 시작 (Recommended)

본 프로젝트는 AI 에이전트를 통한 **자동 환경 구축**을 지원합니다. 레포지토리를 클론한 후, AI 에이전트(Claude Code 등)에게 다음과 같이 명령하세요:

> **"hugo-env-setup 스킬을 사용해서 블로그 포스팅을 위한 초기 환경을 자동으로 구축해줘."**

이 명령 한 번으로 Hugo 설치, 테마 동기화, Git 설정 및 MCP 인증이 자동으로 완료됩니다.

---

## 🚀 Quick Start: 수동 환경 설정 (Manual)

블로그 기여를 위해 로컬 환경에 **Hugo Extended 버전**이 설치되어 있어야 합니다.

### Mac
```bash
brew install hugo
```

### Windows
```powershell
winget install Hugo.Hugo.Extended
```

### 필수: 테마 서브모듈 초기화
레포지토리 클론 후 반드시 아래 명령어를 실행하여 테마 파일을 가져와야 합니다.
```bash
git submodule update --init --recursive
```

---

## ✍️ 포스팅 방법 (Workflow)

모든 작업은 `main` 브랜치를 기준으로 진행됩니다.

1.  **새 포스트 생성** (날짜 prefix와 영문 kebab-case 파일명 권장)
    ```bash
    hugo new posts/2026-03-24-my-new-post.md
    ```
2.  **콘텐츠 작성 및 메타데이터 설정**
    - `content/posts/파일명.md` 파일을 열어 내용을 작성합니다.
    - **중요**: 상단 Front Matter에서 `draft: false`로 설정해야 배포됩니다.
3.  **로컬 미리보기**
    ```bash
    hugo server
    ```
    - `http://localhost:1313` 에서 실물 확인
4.  **배포 (Push)**
    ```bash
    git add .
    git commit -m "post: 새로운 글 추가"
    git push origin main
    ```
    - `main` 브랜치에 푸시하면 GitHub Actions가 1~2분 내로 실서비스에 배포합니다.

---

## 🤖 AI 에이전트 협업 가이드 (Skills)

본 프로젝트는 AI 코딩 어시스턴트(Claude Code 등)와 함께 원활하게 협업할 수 있도록 **전용 스킬(Skills)**이 내장되어 있습니다. 이 스킬들은 프로젝트의 복잡한 규칙(Front Matter, 정렬, 스타일 등)을 AI가 자동으로 준수하도록 돕습니다.

### 사용 가능한 스킬 (Available Skills)
- **`hugo-blog-writer`**: `.agents/skills/hugo-blog-writer/` 하위에 위치. 기술 블로그 포스팅 전문 작가 스킬입니다.
- **`hugo-env-setup`**: `.agents/skills/hugo-env-setup/` 하위에 위치. 로컬 환경 자동 구축 및 검증을 위한 자동화 스킬입니다. (처음 프로젝트 시작 시 AI 에이전트에게 **"hugo-env-setup 스킬 써서 초기 환경 다 잡아줘"**라고 요청하세요.)

### AI에게 글 작성을 요청하는 법 (Usage)
AI 에이전트(예: 클로드코드)에게 아래와 같이 요청하면 내장된 스킬을 바탕으로 고품질의 초안을 생성합니다.

- **방법 1 (스킬 이름 직접 호출 - 추천)**
  > "[주제]에 대해 우리 **hugo-blog-writer 스킬**을 써서 블로그 글 하나 써줘. 소스 데이터는 이 파일이야: [파일명]"
- **방법 2 (일반 요청 - 자동 감지)**
  > "NSoft America 블로그용 기술 포스트 하나 작성해줘. 주제는 [주제]야." (AI가 프로젝트 맥락을 읽어 자동으로 스킬 로딩)

### 스킬을 통해 자동 보장되는 품질
- **포맷 안정성**: 배포 에러가 없는 **YAML(`---`)** 형식의 Front Matter 자동 생성.
- **레이아웃 일관성**: 모든 포스트가 상자(카드) 스타일로 예쁘게 정렬되도록 **특수 레이아웃 옵션** 자동 적용.
- **규정 준수**: 날짜 규칙, 파일명 컨벤션, 필수 섹션(Overview, Background 등)을 AI가 알아서 지킵니다.

---

## 📌 주요 컨벤션 (Conventions)

- **Front Matter**: 반드시 **YAML(`---`)** 형식을 사용하세요.
- **이미지**: `static/images/` 폴더에 저장 후 `![alt](/images/filename.png)` 형식으로 참조하세요.
- **레이아웃**: 홈 화면 카드 정렬을 위해 `weight` 속성 사용을 권장합니다.

상세한 기여 가이드는 [NSoft America 기술 블로그 기여 가이드](https://nsoft-america-inc.github.io/posts/2026-03-24-nsoft-america-blog-guide/)를 참조하세요.

---

## 📂 프로젝트 구조
- `archetype/`: 새 포스트 생성 시 적용되는 기본 템플릿
- `content/`: 블로그 포스트 및 정적 페이지(About 등) 마크다운 파일
- `static/`: 이미지, 폰트 등 정적 리소스
- `themes/`: PaperMod 테마 (서브모듈)
- `hugo.toml`: 사이트 전역 설정 파일

---

## 🤝 기여 안내
본 블로그는 NSoft America 팀원 모두에게 열려 있습니다. 문의사항이나 권한 요청은 조직 관리자에게 연락 바랍니다.

&copy; 2026 NSoft America. Powered by Hugo & PaperMod.
