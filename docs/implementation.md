# Kernel extensions
LWP branch: thread creation/join과 process lifecycle, 공유 주소공간의 자원 해제 순서를 함께 다뤄야 합니다. 기존 결과는 FAIL이며 미완성으로 남습니다.
COW branch: fork의 물리 페이지 공유, write fault 시 복사, page reference accounting이 핵심입니다. 기존 PASS는 SOURCE_REPORTED입니다.
각 branch Makefile 및 MIT LICENSE를 보존합니다. kernel boot/QEMU 검증을 이번에 실행하지 않았습니다. COW PASS를 LWP로 전파하거나 두 branch를 통합 성공한 것으로 서술하지 않습니다.

