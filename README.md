# xv6 운영체제 확장: LWP와 Copy-on-Write

운영체제 수업에서 MIT xv6를 바탕으로 스레드 수명 관리와 가상 메모리 공유를 구현한 프로젝트입니다. 두 과제는 각각 독립된 커널이며 프로세스·주소공간·물리 페이지가 언제 만들어지고 해제되는지에 초점을 맞췄습니다.

## 사용 기술

C, x86 assembly, xv6, Make, QEMU를 사용합니다. 기본 커널은 MIT xv6이고, 여기서는 LWP와 Copy-on-Write 확장을 다룹니다.

## LWP: 주소공간을 공유하는 실행 흐름

[xv6-public-project03](xv6-public-project03)의 `proc.c`에 `thread_create`, `thread_join` 경로가 있습니다. 프로세스 생성과 달리 주소공간을 공유하면서 시작점과 스택, 종료 값을 관리해야 합니다.

생성 함수만 추가해서는 충분하지 않습니다. 종료 시 다른 스레드가 쓰는 주소공간을 먼저 해제하지 않아야 하고 join과 자원 해제 순서도 맞아야 합니다. 당시 LWP는 테스트를 통과하지 못했으며 미완성으로 남았습니다. 특정 한 원인으로 실패를 단정하지 않고 생성·종료·대기의 연결을 읽을 수 있는 구현으로 보존했습니다.

## COW: 실제로 쓸 때만 페이지 복사

[xv6-public-project04](xv6-public-project04)는 `fork`에서 부모 메모리를 곧바로 전부 복사하는 비용을 줄이는 과제입니다.

1. `vm.c`의 `copyuvm`에서 부모와 자식이 물리 페이지를 공유하도록 매핑합니다.
2. 쓰기 권한을 조정하고 `kalloc.c`의 `pagerefc`로 참조 수를 관리합니다.
3. 페이지 fault가 발생하면 `trap.c`에서 `CoW_handler`로 연결합니다.
4. 공유 중인 페이지는 복사하고, 단독 참조라면 쓰기 가능한 상태로 바꿉니다.

복사를 늦추는 것뿐 아니라 참조 수와 페이지 해제를 맞추는 것이 핵심입니다. 당시 과제 기록에서 COW는 테스트 통과로 남아 있습니다. 이를 LWP와 합친 통합 커널의 성공으로 해석하지는 않습니다.

## 코드를 읽는 순서

COW는 `fork → copyuvm → page fault → CoW_handler → kfree` 순서로 보면 공유와 해제가 연결됩니다. LWP는 `proc.c`의 create/join과 종료 처리부터 보는 편이 좋습니다.

## 빌드와 실행

x86용 C 개발 도구, Make, QEMU를 준비하고 원하는 과제에서 실행합니다.

```bash
cd xv6-public-project04
make qemu-nox
```

컴파일러 접두사와 QEMU 경로는 Makefile에 맞춥니다. 두 과제의 소스를 한 커널로 섞지 않습니다.

xv6 원저자 표기와 각 디렉터리의 MIT LICENSE를 유지합니다. [구현 노트](docs/implementation.md) · [출처](ATTRIBUTION.md)
