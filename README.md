# xv6: Lightweight Processes and Copy-on-Write
MIT xv6를 기반으로 두 운영체제 기능을 별도 과제로 구현한 저장소입니다. xv6 자체는 upstream 프로젝트이며, 이 저장소의 학습 기여는 kernel 확장 코드입니다.

## 두 실험
- `xv6-public-project03`: LWP 및 thread_create/thread_join 경로. 기존 기록은 **Incomplete / Test Fail**입니다.
- `xv6-public-project04`: fork 시 페이지 공유와 쓰기 시 복사를 다루는 COW 확장. 기존 기록은 **Complete / Test Pass (SOURCE_REPORTED)**입니다.

두 디렉터리는 합쳐진 하나의 검증된 kernel release가 아닙니다. 주소공간·프로세스 수명 관리와 페이지 참조 관리라는 서로 다른 문제를 보여줍니다.

## 실행
각 과제 디렉터리의 Makefile을 사용합니다. x86 개발 도구, make 및 QEMU가 필요합니다. 일반적인 실행은 해당 디렉터리에서 `make qemu-nox`이며 toolchain 설정은 Makefile을 확인하세요.

## 검증 및 한계
확인한 것은 source/license 존재와 빌드 구조를 확인합니다. 노트북에 적합한 C/QEMU toolchain이 없어 kernel 부팅 테스트는 재실행하지 않았습니다. 과거 COW PASS를 REPRODUCED로 올리지 않습니다. LWP 실패도 숨기지 않습니다.

각 디렉터리의 MIT LICENSE와 원저자 표기는 그대로 유지됩니다. [구현 노트](docs/implementation.md)에 syscall과 메모리 경계를 정리했습니다.
