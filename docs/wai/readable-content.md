# 읽기 쉬운 콘텐츠 가이드라인

## 목차
1. [읽기 쉬운 콘텐츠의 중요성](#읽기-쉬운-콘텐츠의-중요성)
2. [텍스트 구성](#텍스트-구성)
3. [가독성 향상](#가독성-향상)
4. [다국어 지원](#다국어-지원)
5. [구현 가이드](#구현-가이드)

## 읽기 쉬운 콘텐츠의 중요성

읽기 쉬운 콘텐츠는 모든 사용자가 웹 콘텐츠를 쉽게 이해하고 활용할 수 있도록 돕습니다.

### 주요 고려사항
- 명확한 문장 구조
- 적절한 글자 크기와 간격
- 일관된 레이아웃
- 적절한 색상 대비

## 텍스트 구성

### 1. 문단 구성
```html
<article class="readable-content">
  <h1>주요 제목</h1>
  
  <p class="lead">
    핵심 내용을 요약하는 리드 문단입니다.
  </p>
  
  <h2>부제목</h2>
  <p>
    명확하고 간단한 문장으로 구성된 본문 내용입니다.
    한 문단에는 하나의 주제만 다룹니다.
  </p>
  
  <h3>세부 제목</h3>
  <ul>
    <li>중요한 포인트는 목록으로 정리</li>
    <li>간단명료한 표현 사용</li>
  </ul>
</article>

<style>
.readable-content {
  max-width: 65ch; /* 한 줄의 적정 길이 */
  line-height: 1.6;
  margin: 0 auto;
}

.lead {
  font-size: 1.2em;
  margin-bottom: 1.5em;
}

p {
  margin-bottom: 1em;
}

h1, h2, h3 {
  margin-top: 1.5em;
  margin-bottom: 0.5em;
}
</style>
```

### 2. 글자 크기와 간격
```css
/* 기본 타이포그래피 설정 */
:root {
  --base-font-size: 16px;
  --scale-ratio: 1.25;
}

body {
  font-size: var(--base-font-size);
  line-height: 1.6;
}

/* 반응형 글자 크기 */
@media screen and (min-width: 768px) {
  :root {
    --base-font-size: 18px;
  }
}

/* 제목 크기 계층 */
h1 { font-size: calc(var(--base-font-size) * var(--scale-ratio) * var(--scale-ratio) * var(--scale-ratio)); }
h2 { font-size: calc(var(--base-font-size) * var(--scale-ratio) * var(--scale-ratio)); }
h3 { font-size: calc(var(--base-font-size) * var(--scale-ratio)); }
```

## 가독성 향상

### 1. 텍스트 스타일
```css
/* 가독성 최적화 스타일 */
.readable-text {
  font-family: -apple-system, BlinkMacSystemFont, 
               "Segoe UI", Roboto, Oxygen-Sans, 
               Ubuntu, Cantarell, sans-serif;
  
  /* 글자 간격 */
  letter-spacing: 0.01em;
  word-spacing: 0.05em;
  
  /* 단락 스타일 */
  max-width: 65ch;
  padding: 1rem;
  
  /* 텍스트 정렬 */
  text-align: left;
  hyphens: auto;
}

/* 강조 텍스트 */
.emphasis {
  font-weight: 600;
  color: #2c5282;
}

/* 인용문 스타일 */
blockquote {
  margin: 1.5em 0;
  padding: 1em;
  border-left: 4px solid #4a5568;
  background-color: #f7fafc;
}
```

### 2. 콘텐츠 구조화
```html
<article class="structured-content">
  <header>
    <h1>문서 제목</h1>
    <p class="meta">
      작성일: <time datetime="2024-03-27">2024년 3월 27일</time>
    </p>
  </header>

  <nav class="table-of-contents">
    <h2>목차</h2>
    <ul>
      <li><a href="#section1">첫 번째 섹션</a></li>
      <li><a href="#section2">두 번째 섹션</a></li>
    </ul>
  </nav>

  <section id="section1">
    <h2>첫 번째 섹션</h2>
    <p>섹션 내용...</p>
  </section>

  <section id="section2">
    <h2>두 번째 섹션</h2>
    <p>섹션 내용...</p>
  </section>
</article>
```

## 다국어 지원

### 1. 언어 설정
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>다국어 지원 페이지</title>
</head>
<body>
  <p>한국어 콘텐츠</p>
  
  <p lang="en">English content</p>
  
  <p lang="ja">日本語のコンテンツ</p>
</body>
</html>
```

### 2. 다국어 타이포그래피
```css
/* 언어별 최적화된 폰트 스택 */
:lang(ko) {
  font-family: "Noto Sans KR", sans-serif;
  word-break: keep-all;
}

:lang(ja) {
  font-family: "Noto Sans JP", sans-serif;
}

:lang(zh) {
  font-family: "Noto Sans SC", sans-serif;
}

/* 방향성 고려 */
[dir="rtl"] {
  text-align: right;
}
```

## 구현 가이드

### 1. 텍스트 크기 조절
```html
<div class="text-size-controls">
  <button onclick="adjustTextSize('decrease')" 
          aria-label="글자 크기 줄이기">
    A-
  </button>
  
  <button onclick="adjustTextSize('increase')"
          aria-label="글자 크기 늘리기">
    A+
  </button>
</div>

<script>
function adjustTextSize(action) {
  const root = document.documentElement;
  const currentSize = parseFloat(
    getComputedStyle(root).getPropertyValue('--base-font-size')
  );
  
  const newSize = action === 'increase' 
    ? currentSize * 1.1 
    : currentSize * 0.9;
    
  root.style.setProperty('--base-font-size', `${newSize}px`);
}
</script>
```

### 2. 가독성 모드
```javascript
class ReadabilityMode {
  constructor() {
    this.isEnabled = false;
    this.setupToggle();
  }

  setupToggle() {
    const toggle = document.createElement('button');
    toggle.textContent = '가독성 모드';
    toggle.onclick = () => this.toggle();
    document.body.appendChild(toggle);
  }

  toggle() {
    this.isEnabled = !this.isEnabled;
    document.body.classList.toggle('readability-mode');
    
    if (this.isEnabled) {
      this.applyReadabilityStyles();
    } else {
      this.removeReadabilityStyles();
    }
  }

  applyReadabilityStyles() {
    document.body.style.setProperty('--line-height', '1.8');
    document.body.style.setProperty('--paragraph-spacing', '1.5em');
    // 추가 가독성 스타일 적용
  }
}
```

## 체크리스트

### 기본 가독성
- [ ] 적절한 글자 크기 사용
- [ ] 충분한 줄 간격
- [ ] 적절한 단락 구분
- [ ] 명확한 제목 구조

### 콘텐츠 구조
- [ ] 논리적인 정보 구조
- [ ] 일관된 스타일 적용
- [ ] 적절한 여백 사용
- [ ] 시각적 계층 구조

### 다국어 지원
- [ ] 언어 태그 적용
- [ ] 언어별 최적화된 폰트
- [ ] 방향성 고려
- [ ] 문자 인코딩 설정

## 참고 자료
- [WCAG 2.1 가독성 지침](https://www.w3.org/WAI/WCAG21/Understanding/readable)
- [웹 타이포그래피 모범 사례](https://www.w3.org/WAI/WCAG21/Understanding/text-spacing.html)
- [다국어 웹 디자인 가이드](https://www.w3.org/International/)