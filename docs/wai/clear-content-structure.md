# 명확한 콘텐츠 구조 가이드라인

## 목차
1. [콘텐츠 구조의 중요성](#콘텐츠-구조의-중요성)
2. [HTML 시맨틱 마크업](#html-시맨틱-마크업)
3. [제목과 섹션](#제목과-섹션)
4. [내비게이션](#내비게이션)
5. [목록과 표](#목록과-표)
6. [폼 구조화](#폼-구조화)

## 콘텐츠 구조의 중요성

명확한 콘텐츠 구조는 모든 사용자가 웹 페이지의 내용을 쉽게 이해하고 탐색할 수 있도록 돕습니다. 특히 스크린 리더 사용자, 키보드 사용자, 인지적 장애가 있는 사용자들에게 매우 중요합니다.

### 주요 이점
- 정보의 논리적 흐름 제공
- 콘텐츠 탐색 용이성
- 스크린 리더 호환성 향상
- 검색엔진 최적화(SEO) 개선

## HTML 시맨틱 마크업

### 1. 기본 페이지 구조
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>페이지 제목</title>
</head>
<body>
  <header>
    <nav>
      <!-- 내비게이션 내용 -->
    </nav>
  </header>
  
  <main>
    <article>
      <!-- 주요 콘텐츠 -->
    </article>
  </main>
  
  <footer>
    <!-- 푸터 내용 -->
  </footer>
</body>
</html>
```

### 2. 주요 시맨틱 요소
- `<header>`: 소개 콘텐츠
- `<nav>`: 내비게이션
- `<main>`: 주요 콘텐츠
- `<article>`: 독립적인 콘텐츠
- `<section>`: 관련 콘텐츠 그룹
- `<aside>`: 부가 콘텐츠
- `<footer>`: 마무리 콘텐츠

## 제목과 섹션

### 1. 제목 구조
```html
<h1>웹사이트 주제</h1>
<section>
  <h2>주요 섹션</h2>
  <section>
    <h3>하위 섹션</h3>
    <h4>상세 내용</h4>
  </section>
</section>
```

### 2. 제목 레벨 지침
- 페이지당 하나의 `<h1>` 사용
- 논리적 순서로 제목 레벨 구성
- 레벨 건너뛰기 금지
- 시각적 스타일과 구조적 레벨 구분

## 내비게이션

### 1. 메인 내비게이션
```html
<nav aria-label="메인 메뉴">
  <ul>
    <li><a href="/">홈</a></li>
    <li>
      <a href="/products">제품</a>
      <ul>
        <li><a href="/products/new">신제품</a></li>
        <li><a href="/products/best">인기제품</a></li>
      </ul>
    </li>
  </ul>
</nav>
```

### 2. 보조 내비게이션
- 건너뛰기 링크
- 브레드크럼
- 페이지 내 목차
- 관련 링크

## 목록과 표

### 1. 목록 구조화
```html
<!-- 순서 없는 목록 -->
<ul>
  <li>항목 1</li>
  <li>항목 2</li>
</ul>

<!-- 순서 있는 목록 -->
<ol>
  <li>첫 번째 단계</li>
  <li>두 번째 단계</li>
</ol>

<!-- 정의 목록 -->
<dl>
  <dt>용어</dt>
  <dd>용어 설명</dd>
</dl>
```

### 2. 표 구조화
```html
<table>
  <caption>2024년 분기별 매출</caption>
  <thead>
    <tr>
      <th scope="col">분기</th>
      <th scope="col">매출액</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">1분기</th>
      <td>1,000만원</td>
    </tr>
  </tbody>
</table>
```

## 폼 구조화

### 1. 기본 폼 구조
```html
<form>
  <fieldset>
    <legend>개인정보</legend>
    
    <div class="form-group">
      <label for="name">이름</label>
      <input type="text" id="name" name="name" required>
    </div>
    
    <div class="form-group">
      <label for="email">이메일</label>
      <input type="email" id="email" name="email" required>
    </div>
  </fieldset>
  
  <button type="submit">제출</button>
</form>
```

### 2. 폼 요소 그룹화
- `<fieldset>`과 `<legend>` 사용
- 관련 입력 필드 그룹화
- 명확한 레이블 제공
- 오류 메시지 연결

## 구현 체크리스트

### 기본 구조
- [ ] 적절한 HTML5 시맨틱 요소 사용
- [ ] 논리적인 제목 구조
- [ ] 명확한 내비게이션 구조
- [ ] 적절한 ARIA 레이블 사용

### 콘텐츠 구성
- [ ] 관련 콘텐츠 그룹화
- [ ] 명확한 정보 계층 구조
- [ ] 일관된 내비게이션 패턴
- [ ] 적절한 표와 목록 구조

### 접근성 검사
- [ ] 제목 구조 검사
- [ ] 내비게이션 접근성 검사
- [ ] 폼 레이블 검사
- [ ] ARIA 사용 적절성 검사

## 참고 자료
- [HTML5 시맨틱 요소](https://developer.mozilla.org/ko/docs/Web/HTML/Element)
- [WAI-ARIA 실습](https://www.w3.org/WAI/ARIA/apg/)
- [웹 접근성 이니셔티브 문서](https://www.w3.org/WAI/fundamentals/)