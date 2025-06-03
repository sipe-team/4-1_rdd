기존에 있던 이미지는 최적화 필요하고, 이를 클라이언트 측에서 처리할 수 있는 방안이 필요

### 이미지 리사이징 방식: Canvas API

- HTML5에서 제공하는 `<canvas>` 요소를 이용해 그래픽을 그리기위한 수단 제공
- 그래픽 뿐만 아니라 애니메이션, 게임 그래픽, 데이터 시각화, 사진 조작 및 실시간 비디오 처리를 위해 사용됨

### 진행한 작업

- Canvas API 기반 이미지 리사이징 구현
  - 기존에 있던 이미지 최적화를 위해, **Canvas API**를 활용한 이미지 리사이징 유틸 함수를 구현
  - `createImageBitmap`으로 이미지를 비트맵으로 변환한 뒤, 비율을 유지하며 지정한 최대 크기 내에서 리사이즈 수행
  - 리사이즈된 이미지는 WebP 포맷의 Data URL 형태로 반환

```ts
export async function resizeImageWithCanvas(
  file: File,
  maxWidth = 800,
  maxHeight = 800,
): Promise<string> {
  const bitmap = await createImageBitmap(file)
  const ratio = Math.min(maxWidth / bitmap.width, maxHeight / bitmap.height, 1)
  const width = bitmap.width * ratio
  const height = bitmap.height * ratio

  const canvas = document.createElement('canvas')
  canvas.width = width
  canvas.height = height

  const ctx = canvas.getContext('2d')
  if (!ctx) throw new Error('2D context 얻기 실패')

  ctx.drawImage(bitmap, 0, 0, width, height)

  return canvas.toDataURL('image/webp', 0.8)
}
```

그런데...발표 미션을 들으면서 캔버스 방식 말고 웹 워커 방식이 더 적합하겠다는 판단이 들었음

- Canvas API는 메인 스레드에서 작동하기 때문에, 이미지 용량이 크거나 연산량이 많으면 UI가 일시적으로 멈출 수 있음
- 특히 모바일 환경에서 사용자 경험 저하

### 다음주차 계획

- Web Worker
  - 메인 스레드와 분리된 백그라운드 스레드에서 작업 처리
  - CPU 연산이 많은 작업을 백그라운드에서 처리
  - 메인 스레드와 완전히 분리되기 떄문에 비동지 작업 처리 가능
  - `postMessage`와 `onmessage` 방식의 메시지 기반 통신으로만 데이터 처리

```jsx
const worker = new Worker('worker.js')
worker.postMessage({ imageDataUrl, maxWidth: 800 })
worker.onmessage = e => {
  const resizedDataUrl = e.data.resizedDataUrl
}
```
