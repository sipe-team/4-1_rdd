# API History Bulk Insert 개선 작업

## 작업 내역

### 큐 설계 및 구현

- **일급 컬렉션으로 큐 래핑**
  - `ApiLogQueue` 클래스를 통해 로그 큐를 일급컬렉션화
- **스프링 빈으로 등록**
  - 큐를 `@Component`로 등록하여 싱글톤으로 관리
  - 전역 상태로 주입받아 사용
- **동시성 안전한 큐 구현체**
  - `ConcurrentLinkedQueue`를 사용하여 멀티스레드 환경에서도 안정적인 동작 보장

### 주기적인 Flush 기능 구현

- **10초마다 큐 flush 수행**
  - 큐에 적재된 로그를 10초마다 DB에 bulk insert 함.

- **Flush 방법**
  - 비교: Quartz의 Scheduler 사용 vs ScheduledExecutorService
  - **ScheduledExecutorService** 선택
    - 단일 스레드 기반으로 flush 작업을 순차적으로 처리 가능
    - 간결한 코드 작성 가능, Quartz 의 경우 제공하는 기능이 많아 오버헤드가 더 큼.
    - 추후 확장할때 제약사항이 더 적음

- **서버 종료 시 flush 처리**
  - `@PreDestroy` 또는 `ContextClosedEvent` 활용해 마지막 flush 보장

- **데이터 유실 방지 전략**
  - 데이터 저장에 실패했을 시 어떻게 대응할건지 전략이 필요함.

### 시퀀스 다이어그램
![image](https://github.com/user-attachments/assets/65c4d165-07a2-4f8b-a144-e0e8db415499)


## 다음 주차 목표 (3주차)

1. **동시성 및 부하 테스트**
   - 대량 트래픽 상황에서 flush 지연, 데이터 유실 여부 점검

2. **큐 사이즈 기반 자동 flush 기능 구현**
   - 큐가 일정 건수(예: 1000건)를 초과하면 즉시 flush
