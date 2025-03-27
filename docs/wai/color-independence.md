# 색상 독립성 가이드라인

## 목차
1. [색상 독립성의 이해](#색상-독립성의-이해)
2. [색상 대비](#색상-대비)
3. [색상 이외의 정보 전달 방법](#색상-이외의-정보-전달-방법)
4. [구현 가이드](#구현-가이드)
5. [테스트 및 검증](#테스트-및-검증)

## 색상 독립성의 이해

색상 독립성은 색상만으로 정보를 전달하지 않고, 색맹이나 색약이 있는 사용자도 모든 정보를 인식할 수 있도록 하는 원칙입니다.

### 주요 고려사항
- 색맹 및 색약 사용자
- 흑백 화면 사용자
- 저시력 사용자
- 고령 사용자

## 색상 대비

### 1. 최소 대비율 기준
- 일반 텍스트: 4.5:1
- 큰 텍스트(18pt 이상): 3:1
- 그래픽 요소: 3:1

### 2. 권장 대비율
```css
/* 높은 대비를 위한 CSS 예시 */
.high-contrast {
  /* 검정 배경에 흰색 텍스트 (21:1) */
  background-color: #000000;
  color: #FFFFFF;
}

.sufficient-contrast {
  /* 진한 파란색 배경에 흰색 텍스트 (8:1) */
  background-color: #0000AA;
  color: #FFFFFF;
}
```

### 3. 대비 모드 지원
```css
/* 다크 모드 지원 */
@media (prefers-color-scheme: dark) {
  :root {
    --text-color: #FFFFFF;
    --background-color: #121212;
    --link-color: #BB86FC;
  }
}

/* 고대비 모드 지원 */
@media (forced-colors: active) {
  :root {
    --text-color: CanvasText;
    --background-color: Canvas;
    --link-color: LinkText;
  }
}
```

## 색상 이외의 정보 전달 방법

### 1. 텍스트 레이블
```html
<!-- 잘못된 예시 -->
<div class="status-red">주문 취소됨</div>

<!-- 올바른 예시 -->
<div class="status" aria-label="취소됨">
  <span class="status-icon">❌</span>
  주문 취소됨
</div>
```

### 2. 패턴과 아이콘
```html
<!-- 차트에서 패턴 사용 -->
<svg>
  <pattern id="pattern1" patternUnits="userSpaceOnUse" width="4" height="4">
    <!-- 점선 패턴 -->
    <path d="M-1,1 l2,-2 M0,4 l4,-4 M3,5 l2,-2" />
  </pattern>
  
  <pattern id="pattern2" patternUnits="userSpaceOnUse" width="4" height="4">
    <!-- 대각선 패턴 -->
    <path d="M-1,1 l2,-2 M0,4 l4,-4 M3,5 l2,-2" />
  </pattern>
</svg>
```

### 3. 형태와 위치
```html
<!-- 폼 오류 표시 -->
<div class="form-group">
  <label for="email">이메일</label>
  <input type="email" id="email" 
         aria-invalid="true"
         aria-describedby="email-error">
  <span id="email-error" class="error">
    ⚠️ 올바른 이메일 형식이 아닙니다
  </span>
</div>
```

## 구현 가이드

### 1. 링크 스타일
```css
/* 색상과 밑줄 모두 사용 */
a {
  color: #0066CC;
  text-decoration: underline;
}

/* 호버 상태에서 추가적인 시각적 표시 */
a:hover {
  color: #003366;
  text-decoration: underline;
  outline: 2px solid currentColor;
}

/* 포커스 표시 */
a:focus {
  outline: 2px solid #0066CC;
  outline-offset: 2px;
}
```

### 2. 필수 입력 필드 표시
```html
<label for="name">
  이름
  <span class="required" aria-label="필수 입력">
    * <!-- 색상과 기호 모두 사용 -->
  </span>
</label>
<input type="text" id="name" required>

<style>
.required {
  color: #D00;
  font-weight: bold;
}
</style>
```

### 3. 상태 표시
```html
<button class="status-button" aria-label="완료됨">
  <span class="status-icon">✓</span>
  <span class="status-text">완료</span>
  <span class="status-border"></span>
</button>

<style>
.status-button {
  border: 2px solid currentColor;
  padding: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.status-icon {
  /* 아이콘으로 상태 표시 */
}

.status-border {
  /* 테두리 패턴으로 상태 표시 */
}
</style>
```

## 테스트 및 검증

### 1. 자동화 도구
- WAVE Evaluation Tool
- aXe
- Lighthouse
- Color Contrast Analyzer

### 2. 수동 테스트 방법
- 흑백 모드로 화면 보기
- 색맹 시뮬레이션 도구 사용
- 고대비 모드 테스트
- 다양한 화면 밝기에서 테스트

### 3. 체크리스트
- [ ] 모든 정보가 색상 없이도 이해 가능
- [ ] 충분한 색상 대비 제공
- [ ] 텍스트 레이블 제공
- [ ] 패턴/아이콘 보조 수단 사용
- [ ] 다양한 상태 표시 방법 구현
- [ ] 고대비 모드 지원
- [ ] 포커스 표시 명확성

## 참고 자료
- [WCAG 색상 대비 지침](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html)
- [WebAIM 색상 대비 검사기](https://webaim.org/resources/contrastchecker/)
- [색맹 안전 색상 팔레트](https://davidmathlogic.com/colorblind/)