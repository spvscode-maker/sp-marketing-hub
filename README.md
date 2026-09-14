# 에스피시스템스 마케팅 허브

회사소개서 · 홍보 영상 · 구축 사례 영상 · 공개 자료를 한 페이지에 모은 외부 공유용 사이트입니다.
GitHub Pages로 호스팅되고, 데이터는 이 저장소의 `data/*.json`, 파일은 `files/`에 보관됩니다.

관리자는 사이트 화면에서 직접 수정하고, 그 내용이 이 저장소에 커밋됩니다. 별도 서버나 데이터베이스가 없습니다.

## 담는 것과 담지 않는 것

이 저장소는 **공개**입니다. 링크를 아는 누구나 볼 수 있습니다.

| 담습니다 | 담지 않습니다 |
| --- | --- |
| 회사 개요 · 연락처 | 고객사명, 수주 금액 등 내부 실적 |
| 회사소개서 PDF | 사내 문서 양식 |
| 유튜브 영상 목록 (이미 공개된 영상) | 미공개 자료, 견적서, 계약 문서 |
| 회사 로고 등 외부 배포용 파일 | |

내부 실적과 사내 양식은 사내 전용 대시보드에서 관리합니다.

## 파일 구조

```
index.html          사이트 본체 (단일 파일)
data/company.json   회사 정보
data/videos.json    영상 목록
data/files.json     자료 목록 (파일 경로·크기·설명)
files/              실제 파일 (PDF, 로고 등)
.nojekyll           GitHub Pages가 파일을 그대로 서빙하도록 함
```

## 최초 설치

### 1. 저장소 만들기

GitHub에서 새 저장소를 **Public**으로 만듭니다. 이름은 `sp-marketing-hub`를 권합니다.
README나 .gitignore는 추가하지 마세요 (이미 있습니다).

### 2. 올리기

이 폴더에서:

```bash
git remote add origin https://github.com/<계정>/sp-marketing-hub.git
git branch -M main
git push -u origin main
```

### 3. GitHub Pages 켜기

저장소 → **Settings** → **Pages**

- Source: `Deploy from a branch`
- Branch: `main` / `/ (root)`
- Save

1~2분 뒤 `https://<계정>.github.io/sp-marketing-hub/` 에서 열립니다. 이 주소를 외부에 공유하면 됩니다.

## 관리자 설정

관리자는 자기 브라우저에 GitHub 토큰을 한 번 등록하면, 이후 사이트 화면에서 바로 수정할 수 있습니다.

### 토큰 발급

1. <https://github.com/settings/personal-access-tokens/new> 로 갑니다
2. **Repository access** → `Only select repositories` → 이 저장소만 선택
3. **Permissions** → Repository permissions → **Contents: Read and write**
4. 만료일을 정하고 발급 (만료되면 다시 발급해 재입력)
5. `github_pat_...` 로 시작하는 값을 복사

### 사이트에 등록

1. 사이트 우측 상단 **관리자** 클릭
2. 설정 창에 계정 / 저장소 / 토큰 입력 → **연결 확인**
3. 이후 편집 · 업로드 · 삭제가 가능해집니다

토큰은 그 브라우저의 localStorage에만 저장되고 저장소에는 올라가지 않습니다.
**단, 브라우저를 공용 PC에서 쓰지 마세요.** 개인 업무용 PC에서만 등록하십시오.
권한을 회수하려면 GitHub 토큰 설정에서 해당 토큰을 삭제하면 즉시 무효가 됩니다.

## 관리자가 할 수 있는 일

| 작업 | 방법 | 결과 |
| --- | --- | --- |
| 회사 정보 수정 | 개요 → 회사 개요 → 편집 | `data/company.json` 커밋 |
| 영상 추가 · 수정 · 삭제 | 영상 → ＋ 버튼 / 수정 / 삭제 | `data/videos.json` 커밋 |
| 회사소개서 올리기 | 회사소개서 → ＋ 새 버전 올리기 | `files/` + `data/files.json` 커밋 |
| 로고 · 자료 올리기 | 자료실 → ＋ 버튼 | `files/` + `data/files.json` 커밋 |

수정하면 GitHub Pages가 다시 빌드되므로 **사이트 반영까지 30초~1분** 걸립니다.

## 제약

- **파일 크기 24MB 이하.** GitHub Contents API 한계입니다. 회사소개서 PDF가 더 크면 압축하거나, 영상은 유튜브에 올려 링크로 연결하세요.
- **동시 수정 주의.** 두 명이 같은 항목을 동시에 고치면 나중 저장이 앞선 것을 덮습니다. 커밋 이력이 남으니 복원은 가능합니다.
- **토큰 만료.** 만료되면 저장이 401로 실패합니다. 새로 발급해 다시 등록하세요.

## 데이터를 직접 고치기

사이트를 쓰지 않고 GitHub 웹에서 `data/*.json`을 직접 편집해도 됩니다. 형식만 지키면 됩니다.

`data/videos.json` 예:

```json
{
  "items": [
    {
      "id": "v01",
      "title": "영상 제목",
      "desc": "설명",
      "youtubeId": "R4eVff8bjRs",
      "kind": "promo",
      "order": 10
    }
  ]
}
```

- `kind`: `promo` (홍보) 또는 `result` (구축 사례)
- `order`: 작은 값이 먼저 표시됩니다
- `youtubeId`: 유튜브 주소의 `v=` 뒤 11자

## 복원

모든 수정이 깃 커밋으로 남습니다. 되돌리려면:

```bash
git log --oneline           # 이력 확인
git revert <커밋>            # 특정 수정 취소
git push
```
