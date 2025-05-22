### 진행 상황

- 폼 컴포넌트에 대한 기본 테스트 코드 작성
- 작성하면서 입력 항목이 많고 중복되는 로직이 존재해, 이를 정리하기 위해 공통 폼 입력 함수를 별도로 구성
- 테스트에서는 각 필드가 올바르게 렌더링되는지, 입력 및 검증 로직이 잘 작동하는지, RTL에서 제공하는 [Priority](https://testing-library.com/docs/queries/about/#priority)에 맞게 사용했는지를 중점적으로 확인
- 테스트 환경 구성
  - Jest, React Testing Library 기반 환경 세팅
  - `@testing-library/react`, `@testing-library/jest-dom`, `jest-axe` 도입 → 사용자 상호작용 테스트 + 접근성(a11y) 검증 병행 가능하도록 구성 (Storybook에도 추후에 접근성 관련 테스트를 위한 addon-a11y 설정을 해주었다!)
- 중복 제거와 테스트 가독성
  - 대부분의 입력 필드가 구조적으로 비슷해서, `getByLabelText`, `type`, `click` 등이 반복적으로 등장하게 되어, 이를 줄이기 위해 입력에 대한 공통 인터랙션 함수를 작성

### 다음 주차 목표

- 이번주에 이미지 최적화 관련해서 찾아보질 못해서 찾아볼 예정
- 개선 전 성능 측정 결과 문서화

---

- 참고한 아티클

- [스토리북으로 인터랙션 테스트하기](https://ui.toast.com/posts/ko_20220111)
- Kent C. Dodds 테스팅 관련 블로그 글
  - [Common mistakes with React Testing Library](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)
