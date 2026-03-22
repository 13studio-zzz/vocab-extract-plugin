# vocab-extract

어휘 교재 이미지에서 **굵은 영단어/숙어**와 한국어 뜻을 자동으로 추출하여 마크다운 표로 출력하는 Claude Code 스킬입니다.

## 설치 방법

Claude Code에서 아래 명령어를 순서대로 입력하세요:

```
/plugin marketplace add YOUR_GITHUB_USERNAME/vocab-extract-plugin
/plugin install vocab-extract@vocab-extract-marketplace
```

## 사용 방법

어휘 이미지를 첨부하고 아래처럼 입력하세요:

```
이 이미지에서 단어 표로 만들어줘
```

## 출력 예시

| 영단어/숙어 | 뜻 |
|---|---|
| price | 가격, 가치(를 매기다) |
| be supposed to | ~해야 하다, ~라고 하다 |
| in addition to | ~뿐만 아니라 |

## 추출 규칙

- **포함**: 굵은(Bold) 검정 영단어/숙어
- **제외**: 연한 글씨 파생어 (appreciation, officially 등)
- 굵은 구문도 포함 (be ready to V, rather than 등)
- 뜻 옆 추가 정보는 괄호로 병기
