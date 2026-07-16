# 📚 MkDocs GitHub Pages 배포 가이드

이 저장소는 **MkDocs(Material 테마)**를 사용하여 작성되었으며, 사용자가 `main` 브랜치에 코드를 푸시하면 **GitHub Actions가 이를 감지하여 GitHub Pages로 자동 빌드 및 배포**합니다.

---

## 📂 프로젝트 폴더 구조

프로젝트는 기본 설정인 `docs` 폴더 구조를 따릅니다.

```text
example-mkdocs/
├── .github/
│   └── workflows/
│       └── deploy.yml   # (선택) GitHub Actions 자동 배포 설정 파일
├── docs/                # 실제 문서 소스 폴더
│   ├── index.md         # 메인 홈 화면
│   └── test/
│       └── index.md     # 테스트 페이지
├── mkdocs.yml           # MkDocs 설정 파일
└── README.md            # 본 가이드 파일

```

---

## ⚙️ 핵심 설정 파일들

### 1. `mkdocs.yml` (문서 설정)

```yaml
site_name: Example MKDocs
site_url: [https://sch6393.github.io/example-mkdocs/](https://sch6393.github.io/example-mkdocs/)

theme:
  name: material
  language: ko
  
nav:
  - Home: index.md
  - test:
      - 개요: test/index.md

```

### 2. `.github/workflows/deploy.yml` (자동 배포 스크립트)

`main` 브랜치에 푸시가 발생하면 깃허브 서버가 이 파일을 읽고 자동으로 빌드 과정을 수행합니다.

```yaml
name: Publish MkDocs to GitHub Pages
on:
  push:
    branches:
      - main  # main 브랜치에 푸시될 때 실행

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      
      - name: Install dependencies
        run: |
          pip install mkdocs-material
          
      - name: Deploy
        run: |
          mkdocs gh-deploy --force

```

---

## 🚀 문서 수정 및 자동 배포 순서

앞으로 문서를 업데이트할 때는 딱 **이 과정**만 거치면 배포까지 자동으로 끝납니다.

### 1단계: 로컬에서 문서 작성 및 확인

1. `docs` 폴더 내에 마크다운(`.md`) 파일을 추가하거나 수정합니다.
2. 로컬 서버를 띄워 실시간으로 결과물을 확인해 봅니다.
```bash
mkdocs serve

```


3. 브라우저에서 `http://127.0.0.1:8000` 접속 후 이상이 없는지 검토합니다.

### 2단계: Git 푸시 (끝!)

작업이 완료되면 평소처럼 깃허브에 커밋하고 푸시만 하세요. 나머지는 깃허브가 다 알아서 합니다.

```bash
git add .
git commit -m "docs: 문서 수정 반영"
git push origin main

```

> **💡 푸시 이후 내부적으로 일어나는 일:**
> 1. 푸시 직후 GitHub 저장소의 **Actions** 탭에 가보면 자동 빌드 배포 작업이 돌아가고 있습니다. (약 1분 소요)
> 2. 작업이 완료되면 `gh-pages` 브랜치에 빌드된 정적 파일들이 자동으로 업데이트됩니다.
> 
> 

---

## ⚙️ GitHub Pages 최초 설정 (최초 1회만 수행)

배포가 정상적으로 작동하려면 GitHub 저장소 웹사이트에서 딱 한 번만 아래 설정을 맞춰주어야 합니다.

1. 내 GitHub 저장소의 **Settings** 탭으로 이동합니다.
2. 왼쪽 메뉴에서 **Pages**를 클릭합니다.
3. **Build and deployment** 섹션의 설정을 아래와 같이 맞춥니다:
* **Source**: `Deploy from a branch`
* **Branch**: `gh-pages` / `/ (root)` 👈 **(주의: `main`이 아닌 `gh-pages`여야 테마가 정상 적용됩니다.)**


4. **Save** 버튼을 누릅니다.

> 💡 **주소 접속 확인:**
> 배포 후 약 1~2분 뒤에 `https://sch6393.github.io/example-mkdocs/` 주소로 접속하면 예쁘게 빌드된 문서 사이트를 볼 수 있습니다!
