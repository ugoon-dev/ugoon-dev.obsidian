# 웹 접근성 표준 (WAI - Web Accessibility Initiative)

## 목차
1. [웹 접근성이란?](#웹-접근성이란)
2. [WCAG (Web Content Accessibility Guidelines)](#wcag)
3. [주요 접근성 원칙](#주요-접근성-원칙)
4. [기술적 구현 방법](#기술적-구현-방법)
5. [검사 및 평가](#검사-및-평가)

## 웹 접근성이란?

웹 접근성은 장애인, 고령자 등이 웹 사이트에서 제공하는 정보에 비장애인과 동등하게 접근하고 이해할 수 있도록 보장하는 것을 말합니다.

### 웹 접근성의 중요성
- 정보 접근의 평등성 보장
- 법적 요구사항 준수
- 더 넓은 사용자층 확보
- 검색엔진 최적화(SEO) 향상

## WCAG

WCAG는 W3C의 WAI에서 제정한 웹 콘텐츠 접근성 지침입니다.

### WCAG 2.1 주요 버전
- WCAG 2.0 (2008년)
- WCAG 2.1 (2018년)
- WCAG 2.2 (2023년)

### 준수 수준
- Level A (최소 수준)
- Level AA (표준 수준)
- Level AAA (최고 수준)

## 주요 접근성 원칙

### 1. 인식의 용이성 (Perceivable)
- 대체 텍스트 제공
- 멀티미디어 대체 수단
- 명확한 콘텐츠 구조
- 색상 독립성

### 2. 운용의 용이성 (Operable)
- 키보드 접근성
- 충분한 시간 제공
- 깜빡임 제한
- 쉬운 내비게이션

### 3. 이해의 용이성 (Understandable)
- 읽기 쉬운 콘텐츠
- 예측 가능한 작동
- 오류 방지 및 정정

### 4. 견고성 (Robust)
- 문법 준수
- 웹 표준 호환성
- 보조 기술 지원

## 기술적 구현 방법

### HTML 구조화
```html
<!-- 의미있는 HTML 구조 예시 -->
<header>
  <h1>웹사이트 제목</h1>
  <nav>
    <ul>
      <li><a href="#main">메인 콘텐츠</a></li>
    </ul>
  </nav>
</header>
```

### ARIA (Accessible Rich Internet Applications)
```html
<!-- ARIA 레이블 사용 예시 -->
<button aria-label="메뉴 열기">
  <span class="icon-menu"></span>
</button>
```

### 이미지 접근성
```html
<!-- 의미있는 대체 텍스트 -->
<img src="logo.png" alt="회사 로고">

<!-- 장식용 이미지 -->
<img src="decoration.png" alt="">
```

## 검사 및 평가

### 자동화 도구
- WAVE (Web Accessibility Evaluation Tool)
- aXe
- HTML_CodeSniffer

### 수동 검사 항목
1. 키보드 탐색
2. 스크린리더 호환성
3. 고대비 모드 테스트
4. 텍스트 확대 테스트

### 체크리스트
- [ ] 모든 이미지에 대체 텍스트 제공
- [ ] 키보드로 모든 기능 사용 가능
- [ ] 충분한 색상 대비
- [ ] 명확한 페이지 구조
- [ ] 오류 메시지 명확성

## 참고 자료
- [W3C 웹 접근성 이니셔티브 (WAI)](https://www.w3.org/WAI/)
- [WCAG 2.1 지침](https://www.w3.org/TR/WCAG21/)
- [한국형 웹 콘텐츠 접근성 지침](https://www.wah.or.kr:444/Participation/guide.asp)