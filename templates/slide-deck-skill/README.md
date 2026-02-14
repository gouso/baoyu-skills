# Slide Deck Skill Template

Claude Code 에이전트 스킬용 슬라이드 생성 템플릿.

## Architecture

```
slide-deck-skill/
├── SKILL.md                          # 메인 스킬 정의 (워크플로우)
├── references/
│   ├── base-prompt.md                # 이미지 생성 프롬프트 템플릿
│   ├── outline-template.md           # 아웃라인 구조 + STYLE_INSTRUCTIONS 포맷
│   ├── analysis-framework.md         # 콘텐츠 분석 프레임워크
│   ├── content-rules.md              # 콘텐츠/스타일 규칙
│   ├── design-guidelines.md          # 디자인 가이드라인
│   ├── layouts.md                    # 레이아웃 갤러리
│   ├── dimensions/
│   │   ├── presets.md                # 프리셋 → 차원 매핑
│   │   ├── texture.md               # 텍스처 차원 (clean, grid, organic, pixel, paper)
│   │   ├── mood.md                  # 무드 차원 (professional, warm, cool, vibrant, dark, neutral)
│   │   ├── typography.md            # 타이포그래피 차원 (geometric, humanist, handwritten, editorial, technical)
│   │   └── density.md               # 밀도 차원 (minimal, balanced, dense)
│   └── styles/
│       ├── corporate.md             # 예시: 비즈니스 스타일
│       ├── blueprint.md             # 예시: 기술 스타일
│       └── sketch-notes.md          # 예시: 교육 스타일
├── scripts/
│   ├── merge-to-pptx.ts             # PNG → PPTX 합치기
│   └── merge-to-pdf.ts              # PNG → PDF 합치기
└── prompts/                          # (런타임) 생성된 프롬프트 저장
```

## How It Works

### Pipeline

```
소스 콘텐츠
  ↓
[분석] 콘텐츠 신호 감지 → 스타일 추천
  ↓
[아웃라인] outline-template.md 포맷으로 outline.md 생성
  │         └── <STYLE_INSTRUCTIONS> 블록 포함 (스타일의 Single Source of Truth)
  ↓
[프롬프트] base-prompt.md + STYLE_INSTRUCTIONS + 슬라이드 콘텐츠 = 프롬프트 파일
  │         └── prompts/01-slide-cover.md, 02-slide-xxx.md, ...
  ↓
[이미지 생성] 각 프롬프트 → AI 이미지 생성 API → PNG
  │            └── ⚠️ 이미지 생성 백엔드는 별도 구성 필요
  ↓
[합치기] PNG들 → PPTX + PDF
```

### Key Design Decisions

| 결정 | 이유 |
|------|------|
| 슬라이드 = 이미지 | 코드 기반 레이아웃보다 AI 이미지가 디자인 품질 훨씬 높음 |
| 4차원 스타일 시스템 | Texture×Mood×Typography×Density = 450가지 조합 가능 |
| STYLE_INSTRUCTIONS = SSOT | 스타일 정보를 한 곳에서 관리, 프롬프트마다 재참조 안 함 |
| 슬라이드별 독립 프롬프트 | 개별 수정/재생성 가능, 디버깅 용이 |
| outline → prompts 2단계 | 전체 구조를 먼저 확정한 뒤 세부 프롬프트 생성 |

## Setup in Your Project

### 1. Copy Template

```bash
cp -r templates/slide-deck-skill/ your-project/skills/your-slide-deck/
```

### 2. Rename & Customize SKILL.md

- `name:` 필드를 프로젝트에 맞게 변경
- `description:` 필드 수정
- Step 7의 이미지 생성 명령을 사용할 API/도구에 맞게 수정

### 3. Configure Image Generation Backend

SKILL.md Step 7에서 이미지 생성 명령을 설정:

```bash
# Gemini Web API 예시
npx -y bun path/to/gemini-web/scripts/main.ts --promptfiles prompts/01-slide-cover.md --image 01-slide-cover.png

# OpenAI DALL-E 예시
# your-image-gen-script --prompt-file prompts/01-slide-cover.md --output 01-slide-cover.png

# 로컬 Stable Diffusion 예시
# sd-generate --from-markdown prompts/01-slide-cover.md --out 01-slide-cover.png
```

### 4. Add/Modify Styles

새 스타일 추가:
1. `references/styles/your-style.md` 생성
2. `references/dimensions/presets.md`에 매핑 추가
3. `SKILL.md`의 Presets 테이블에 추가

### 5. Run Scripts

```bash
# PPTX 생성
npx -y bun scripts/merge-to-pptx.ts slide-deck/topic-slug/

# PDF 생성
npx -y bun scripts/merge-to-pdf.ts slide-deck/topic-slug/
```

## Customization Guide

### Adding More Presets

`references/styles/` 에 `.md` 파일 추가. 필수 섹션:
- Design Aesthetic
- Background
- Typography (Primary + Secondary)
- Color Palette (hex 코드 포함)
- Visual Elements
- Style Rules (Do/Don't)

### Modifying the Base Prompt

`references/base-prompt.md` 수정. 핵심 구조:
1. Image Specifications (고정)
2. Core Principles (고정)
3. Text Style rules (고정)
4. `---`
5. STYLE_INSTRUCTIONS (outline에서 복사)
6. `---`
7. SLIDE CONTENT (슬라이드별 내용)

### Changing Aspect Ratio

16:9 외 비율이 필요하면:
1. `references/base-prompt.md`의 Aspect Ratio 변경
2. `scripts/merge-to-pptx.ts`의 `pptx.layout` 변경
3. 레이아웃 가이드 조정
