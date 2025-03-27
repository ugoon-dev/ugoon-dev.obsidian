# 멀티미디어 대체 수단 가이드라인

## 목차
1. [멀티미디어 대체 수단의 이해](#멀티미디어-대체-수단의-이해)
2. [자막 제공](#자막-제공)
3. [수화 통역](#수화-통역)
4. [오디오 설명](#오디오-설명)
5. [대본 제공](#대본-제공)
6. [구현 가이드](#구현-가이드)

## 멀티미디어 대체 수단의 이해

멀티미디어 대체 수단은 청각 장애인, 시각 장애인, 또는 특정 상황에서 소리를 들을 수 없는 사용자들이 멀티미디어 콘텐츠를 이해할 수 있도록 제공되는 대안적 방법들입니다.

### 대체 수단이 필요한 콘텐츠
- 동영상
- 오디오
- 애니메이션
- 라이브 스트리밍
- 인터랙티브 미디어

## 자막 제공

### 1. 자막의 종류
- 폐쇄 자막 (Closed Caption)
- 개방 자막 (Open Caption)
- 실시간 자막

### 2. 자막 제작 지침
- 화자 구분을 위한 색상 사용
- 적절한 글자 크기와 위치
- 배경과 구분되는 색상
- 적절한 표시 시간

### 3. 기술적 구현
```html
<!-- WebVTT 자막 파일 연결 -->
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track kind="captions" src="captions.vtt" srclang="ko" label="한국어" default>
</video>
```

## 수화 통역

### 1. 수화 통역 제공 방식
- 영상 내 수화 통역사 삽입
- 별도 수화 통역 영상 제공
- 선택적 수화 통역 화면

### 2. 수화 통역 화면 요구사항
- 적절한 화면 크기
- 명확한 시인성
- 통역사의 상반신 포함
- 배경과의 명확한 구분

### 3. 구현 예시
```html
<!-- 수화 통역 영상 제공 -->
<div class="video-container">
  <video id="main-video" controls>
    <source src="main-video.mp4" type="video/mp4">
  </video>
  <video id="sign-language" controls>
    <source src="sign-language.mp4" type="video/mp4">
  </video>
</div>
```

## 오디오 설명

### 1. 오디오 설명의 필요성
- 시각적 정보의 음성 설명
- 장면 전환 설명
- 비언어적 행동 설명
- 화면에 표시되는 텍스트 읽기

### 2. 오디오 설명 제작 지침
- 간결하고 명확한 설명
- 중요 시각 정보 우선 순위
- 원본 오디오와 겹치지 않도록 타이밍 조절
- 객관적 설명 제공

### 3. 구현 방법
```html
<!-- 오디오 설명이 포함된 대체 영상 제공 -->
<video controls>
  <source src="video-with-description.mp4" type="video/mp4">
  <source src="video-without-description.mp4" type="video/mp4">
  <button class="audio-description-toggle">오디오 설명 켜기/끄기</button>
</video>
```

## 대본 제공

### 1. 대본의 구성요소
- 대사
- 화자 정보
- 배경음악 설명
- 효과음 설명
- 장면 설명

### 2. 대본 작성 지침
- 시간 정보 포함
- 단락 구분
- 검색 가능한 텍스트 형식
- 다운로드 가능한 형식 제공

### 3. 대본 제공 방식
```html
<!-- 접이식 대본 제공 -->
<div class="transcript-container">
  <button aria-expanded="false" aria-controls="transcript">
    대본 보기
  </button>
  <div id="transcript" hidden>
    <h3>영상 대본</h3>
    <!-- 대본 내용 -->
  </div>
</div>
```

## 구현 가이드

### 1. 플레이어 접근성
- 키보드 조작 지원
- 높은 대비의 컨트롤
- 스크린리더 호환성
- 자동 재생 방지

### 2. 반응형 디자인
```css
.video-container {
  position: relative;
  width: 100%;
  padding-bottom: 56.25%; /* 16:9 비율 */
}

.video-container video {
  position: absolute;
  width: 100%;
  height: 100%;
}

@media (min-width: 768px) {
  .video-container.with-sign-language {
    display: grid;
    grid-template-columns: 70% 30%;
  }
}
```

### 3. 성능 최적화
- 적절한 비디오 포맷 선택
- 다양한 해상도 제공
- 프리로딩 설정
- 대역폭 고려

## 체크리스트

### 필수 항목
- [ ] 자막 제공
- [ ] 대본 제공
- [ ] 키보드 접근성
- [ ] 플레이어 컨트롤 접근성

### 권장 항목
- [ ] 수화 통역 제공
- [ ] 오디오 설명 제공
- [ ] 다국어 자막 지원
- [ ] 자막 스타일 사용자 정의 옵션

## 참고 자료
- [WCAG 2.1 멀티미디어 가이드라인](https://www.w3.org/WAI/WCAG21/Understanding/time-based-media)
- [WebVTT 파일 형식](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API)
- [HTML5 비디오 접근성](https://developer.mozilla.org/en-US/docs/Web/Guide/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video)