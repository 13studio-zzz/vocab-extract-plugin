---
name: vocab-extract
description: >
  Extracts bold English vocabulary words/phrases and their Korean meanings from vocabulary
  textbook images and outputs a clean markdown table. Use this skill whenever the user
  attaches one or more vocabulary images (textbook screenshots, vocabulary list photos,
  word list images) and asks to extract words, make a table, build an Excel sheet, or
  organize vocabulary. Trigger even if the user just says "이미지에서 단어 뽑아줘",
  "표로 만들어줘", "엑셀로 만들어줘", or pastes vocabulary images without much explanation.
  This skill is especially valuable for Korean English-learner vocabulary books where
  bold words appear alongside Korean definitions.
---

# Vocabulary Extractor

Your job is to read vocabulary images and extract only the **PRIMARY headwords** (표제어)
with their Korean meanings into a markdown table. Precision matters: you are the source
of truth, so read carefully.

## 이미지 구조 이해 (매우 중요)

이 단어장 이미지는 **[영어단어, 한국어뜻] 섹션**들로 이루어진 표 구조다.
각 섹션은 번호(e.g., 0531, 0532)로 구분된다.

### 핵심 추출 규칙: 섹션 내 최대 굵기 단어만

각 섹션에서 **가장 크고 굵은 검은색 단어(들)만** 추출한다.

판단 방법: 섹션 안의 영어 단어들을 훑어보고 **"이 단어가 이 섹션에서 가장 굵은가?"** 를 묻는다.
- YES → 추출 ✓
- NO (더 굵은 단어가 같은 섹션에 있음) → 제외 ✗

두 단어 이상이 **시각적으로 동일한 최대 크기/굵기**를 공유하면, 그 모두를 추출한다.

**예)**
- `bound` 섹션: bound가 가장 굵음 → bound만 추출. bind/bond/band는 bound보다 작으므로 제외
- `college` 섹션: college와 colleague가 동일한 최대 굵기 → 둘 다 추출
- `crime` 섹션: crime이 가장 굵음 → crime만 추출. criminal/crisis는 더 작으므로 제외

> ⚠️ **색상 조건 — 반드시 검은색(black)이어야 한다**
> 초록색(green), 회색(gray) 등 **검은색이 아닌 영어 단어는 굵기와 무관하게 절대 추출하지 않는다.**

### 추가 제외 규칙: 굵기와 무관하게 항상 제외

굵어 보이더라도 아래 유형은 표제어가 아니므로 절대 추출하지 않는다.

**① 문법 플레이스홀더가 포함된 구문**

`V`, `N`, `O`, `to V`, `V-ing`, `~` 등 문법 기호가 들어간 구문은 사용 예문이지 표제어가 아니다.

- `be bound to V` → 제외 ✗
- `in an effort to V` → 제외 ✗

⚠️ `in charge of`, `deal with`처럼 문법 기호 없이 고정된 숙어는 이 규칙에 해당하지 않는다. 굵기로 판단한다.

**② 섹션 대표 표제어의 직접 파생어**

접사(-ary, -tion, -ity, -less, -ness 등)로 형태가 변형된 파생어는 제외한다.

- `boundary` (bound + -ary) → 제외 ✗
- `limitation` (limit + -ation) → 제외 ✗

**③ 오른쪽 열 단어들**

페이지 오른쪽 열에 있는 단어들은 굵기와 관계없이 절대 추출하지 않는다.

## 추출 방법

1. **열(column)을 파악한다**: 페이지를 왼쪽 열과 오른쪽 열로 구분한다
2. **섹션을 파악한다**: 번호(0531, 0532...)로 구분된 섹션들을 찾는다
3. **각 섹션에서 PRIMARY를 찾는다**: 가장 크고 굵은 검은 볼드 영어 단어들을 식별한다
4. **같은 행의 뜻을 찾는다**: PRIMARY 단어와 동일한 행에 있는 한국어 뜻을 가져온다
5. **초록 얇은 글씨 확인**: 뜻 바로 우측에 초록 얇은 글씨가 있으면 뜻에 포함한다

## 예시

**섹션 0531** (PRIMARY 1개):
- `effort` (PRIMARY, 크고 굵은 볼드) → `노력, 시도` ✓
- `make an effort` (관련어, 작음) → 추출 제외 ✗
- `in an effort to V` (관련어, 작음) → 추출 제외 ✗

**섹션 0532** (PRIMARY 1개):
- `electrical` (PRIMARY, 크고 굵은 볼드) → `전기의` ✓
- `electricity` (관련어, 작음) → 추출 제외 ✗
- `electric` (관련어, 작음) → 추출 제외 ✗
- `electronic` (관련어, 작음) → 추출 제외 ✗

**섹션 0537** (PRIMARY 2개 — 둘 다 추출):
- `hurt` (PRIMARY) → `다치게 하다 (hurt : hurt : hurt)` ✓ ← 초록 얇은 글씨 포함
- `smart` (PRIMARY) → `영리한, 아프다` ✓
- `hurtful` (관련어) → 추출 제외 ✗

**섹션 0538** (PRIMARY 2개 — 둘 다 추출):
- `literally` (PRIMARY) → `문자 그대로, 정말로` ✓
- `literature` (관련어) → 추출 제외 ✗
- `letter` (PRIMARY) → `편지, 글자` ✓
- `capital letter` (관련어) → 추출 제외 ✗

**섹션 0539** (PRIMARY 2개 — 둘 다 추출):
- `male` (PRIMARY) → `남성(의)` ✓
- `female` (PRIMARY) → `여성(의)` ✓

**섹션 0461** (PRIMARY 1개):
- `fire` (PRIMARY) → `불, 사격[해고]하다` ✓
- `firefighter` (관련어) → 추출 제외 ✗
- `fireman` (관련어) → 추출 제외 ✗
- `fireplace` (관련어) → 추출 제외 ✗

**섹션 0610** (PRIMARY 1개):
- `bound` (섹션 최대 굵기) → `묶인, 뛰다, 범위` ✓
- `be bound to V` (V 포함 → 제외) ✗
- `boundary` (bound 파생어 → 제외) ✗
- `bind` (bound보다 작음 → 제외) ✗
- `bond` (bound보다 작음 → 제외) ✗
- `band` (bound보다 작음 → 제외) ✗

**섹션 0611** (PRIMARY 2개 — 동일 최대 굵기):
- `college` (최대 굵기) → `대학` ✓
- `colleague` (college와 동일한 최대 굵기) → `동료` ✓

**섹션 0376** (PRIMARY 1개):
- `limited` (섹션 최대 굵기) → `제한된` ✓
- `limit` (limited보다 작음 → 제외) ✗
- `limitation` (얇음 → 제외) ✗
- `unlimited` (얇음 → 제외) ✗

## What to exclude (요약)

- 섹션 내 **최대 굵기가 아닌** 모든 단어: e.g., `bind`, `bond`, `band`(bound 섹션에서), `limit`(limited 섹션에서)
- **문법 플레이스홀더** 포함 구문: e.g., `be bound to V`, `make an effort to V`
- 대표 표제어의 **파생어**: e.g., `boundary`, `limitation`, `firefighter`, `electricity`
- **오른쪽 열** 단어들: 굵게 보여도 절대 추출 안 함

## 같은 행의 초록 얇은 글씨 처리 (매우 중요)

페이지에는 여러 줄에 걸쳐 초록색 글씨가 등장할 수 있다. 그 중 **반드시 포함해야 하는 것**과
**절대 포함하면 안 되는 것**을 엄격히 구분한다.

### 포함 규칙: 같은 행(row)에 있는 초록 얇은 글씨만

```
[굵은 검은 단어]  [한국어 뜻]  [초록 얇은 글씨] ← 이것만 포함
```

- 굵은 검은 단어와 **정확히 같은 줄(행)**에 있는 초록 얇은 글씨
- 뜻(한국어) 바로 오른쪽에 위치
- 해당 단어에 대한 보충 정보 (동사 변화형, 품사 표시 등)

### 절대 포함 금지: 다른 행의 초록 글씨

- 위 또는 아래 다른 줄에 있는 초록 글씨는 다른 단어에 속한 정보임
- 행이 조금이라도 다르면 절대 포함하지 않는다
- **"이 초록 글씨가 지금 처리 중인 단어와 같은 줄에 있는가?"** 를 반드시 확인

### 출력 형식

초록 글씨가 있는 경우 뜻 뒤에 괄호로 붙인다:

```
| hold | 잡다, 유지하다 (held held) |
| leave | 떠나다 (left left) |
| gradually | 점차적으로, 차츰 |        ← 초록 글씨 없으면 그냥 뜻만
```

### 판단 기준 체크리스트

처리 중인 단어 행에서 초록 글씨를 찾을 때:
1. 그 초록 글씨가 **같은 행의 뜻 오른쪽**에 있는가? → 포함
2. 그 초록 글씨가 **윗줄 또는 아랫줄**에 있는가? → 제외
3. 확신이 없으면 → 제외 (잘못 포함하는 것이 누락보다 나쁨)

## Output

Output **only** the markdown table below. No introduction, no explanation, no summary.
Nothing before or after the table.

```
| 영단어 | 뜻 |
|---|---|
| effort | 노력, 시도 |
| electrical | 전기의 |
| hurt | 다치게 하다 (hurt : hurt : hurt) |
| smart | 영리한, 아프다 |
| male | 남성(의) |
| female | 여성(의) |
```

If the user provides multiple images, process all of them in sequence and combine into
one table in the order they appear.

## Saving Rule (Excel 파일 생성 시)

### 파일명 규칙

이미지에서 확인된 섹션 번호 범위를 기반으로 파일명을 생성한다:
- 복수 섹션: `단어_0461-0467.xlsx`
- 단일 섹션: `단어_0461.xlsx`
- 섹션 번호 불명: `단어장_YYYYMMDD.xlsx` (오늘 날짜)

### 저장 위치 우선순위 (폴백 전략)

아래 순서대로 시도하여 **최초로 쓰기 가능한 위치**에 저장한다.
각 경로에 폴더가 없으면 자동 생성을 시도하고, 실패 시 다음 순위로 넘어간다.

```
1순위: ~/Desktop/단어장/
2순위: ~/Documents/단어장/
3순위: ~/단어장/          ← 홈 디렉토리 직하
4순위: ./단어장/          ← 현재 작업 디렉토리
5순위: /tmp/단어장/       ← 최후 수단 (재부팅 시 삭제됨, 사용자에게 경고)
```

Python으로 구현 시 다음 패턴을 사용한다:

```python
import os
from pathlib import Path

candidates = [
    Path.home() / "Desktop" / "단어장",
    Path.home() / "Documents" / "단어장",
    Path.home() / "단어장",
    Path.cwd() / "단어장",
    Path("/tmp") / "단어장",
]

save_dir = None
for candidate in candidates:
    try:
        candidate.mkdir(parents=True, exist_ok=True)
        test_file = candidate / ".write_test"
        test_file.touch()
        test_file.unlink()
        save_dir = candidate
        break
    except (PermissionError, OSError):
        continue

if save_dir is None:
    raise RuntimeError("저장 가능한 경로를 찾을 수 없습니다.")
```

### 저장 후 사용자에게 반드시 알릴 것

- 실제 저장된 전체 경로를 출력한다
- 5순위(`/tmp/`)에 저장된 경우 "재부팅 시 삭제됩니다" 경고를 함께 출력한다
- 예시: `저장 완료: /Users/dohyung/Desktop/단어장/단어_0461-0467.xlsx`
