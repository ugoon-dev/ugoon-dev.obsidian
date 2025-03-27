# 텍스트 정렬 가이드라인

## 1. 중앙 정렬 지양

### 가독성 저하 요인
1. **시선 이동의 비효율성**
   - 매 줄마다 시작점이 다름
   - 자연스러운 읽기 패턴 방해
   - 인지 부하 증가

2. **텍스트 블록 인식의 어려움**
   - 불규칙한 여백으로 인한 형태 인식 저하
   - 문단 구분의 모호함
   - 콘텐츠 계층 구조 파악 어려움

### 예외적 사용 케이스
- 짧은 제목
- 한 줄 텍스트
- 버튼 레이블
- 알림 메시지

## 2. F패턴 읽기를 고려한 왼쪽 정렬

### F패턴의 이해
1. **사용자 읽기 패턴**
   - 좌측에서 우측으로 수평 이동
   - 아래로 이동하여 두 번째 수평 이동
   - 좌측을 따라 수직 스캔

2. **효과적인 정보 전달**
   - 중요 정보를 좌측에 배치
   - 단락 첫 문장에 핵심 내용 포함
   - 스캔 가능한 구조 설계

### 구현 가이드라인
```css
.content-block {
    text-align: left;
    max-width: 66ch; /* 가독성을 위한 최적 길이 */
    margin: 0 auto;
    padding: 0 16px;
}

.paragraph {
    margin-bottom: 1.5em;
    line-height: 1.6;
}

.heading {
    margin-top: 2em;
    margin-bottom: 1em;
    font-weight: 600;
}
```

## 3. 행간 관리

### 최적 행간 설정
1. **기본 원칙**
   - 본문 텍스트 150% 이상 유지
   - 모바일 환경 특성 고려
   - 폰트 크기와의 조화

2. **가독성 최적화**
   - 텍스트 블록 간 충분한 여백
   - 시각적 계층 구조 명확화
   - 스캔 가능성 향상

### 구현 예시
```css
/* 기본 텍스트 스타일 */
body {
    font-size: 16px;
    line-height: 1.6; /* 160% */
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
}

/* 제목 스타일 */
h1, h2, h3 {
    line-height: 1.3; /* 130% */
    margin-bottom: 0.5em;
}

/* 본문 단락 */
p {
    margin-bottom: 1.5em;
    line-height: 1.6; /* 160% */
}

/* 목록 항목 */
li {
    line-height: 1.5; /* 150% */
    margin-bottom: 0.5em;
}
```

## 4. 반응형 타이포그래피

### 화면 크기별 최적화
1. **뷰포트 기반 조정**
   - 화면 크기에 따른 동적 폰트 크기
   - 최소/최대 크기 제한
   - 비율 기반 스케일링

2. **레이아웃 고려사항**
   - 여백의 동적 조정
   - 행간의 상대적 유지
   - 가독성 우선 설계

### 구현 예시
```css
/* 반응형 타이포그래피 */
:root {
    --base-font-size: 16px;
    --scale-ratio: 1.2;
}

@media screen and (min-width: 320px) {
    :root {
        --base-font-size: calc(16px + 0.5vw);
    }
}

@media screen and (min-width: 1200px) {
    :root {
        --base-font-size: 20px;
    }
}

body {
    font-size: var(--base-font-size);
    line-height: calc(1.6 * var(--base-font-size));
}
```

## 5. 접근성 고려사항

### 주요 고려사항
1. **텍스트 크기 조정**
   - 사용자 정의 크기 지원
   - 확대/축소 기능 보장
   - 최소 폰트 크기 준수

2. **색상 대비**
   - WCAG 2.1 기준 준수
   - 가독성 확보
   - 다크 모드 대응

## 참고 자료
1. Nielsen Norman Group - F-Shaped Pattern
2. WCAG 2.1 - Text Guidelines
3. Material Design - Typography
4. Apple Human Interface Guidelines - Typography

## 변경 이력
- 2025.03.27: 최초 작성
- 추가 업데이트 예정