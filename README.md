# 1. Top
-> 시스템에서 실행 중인 프로세스의 실시간 모니터링을 제공하는 명령어 
- CPU, 메모리, 프로세스 ID 등의 정보를 표시
- 실행 중인 프로세스의 상태를 확인할 수 있습니다.

#### top 실행 전 옵션

  순간의 정보를 확인하려면 -b 옵션 추가(batch 모드)
  -n : top 실행 주기 설정(반복 횟수)
  top 실행 후 명령어
  shift + p : CPU 사용률 내림차순
  shit + m : 메모리 사용률 내림차순
  shift + t : 프로세스가 돌아가고 있는 시간 순
  k : kill. k 입력 후 PID 번호 작성. signal은 9
  f : sort field 선택 화면 -> q 누르면 RES순으로 정렬
  a : 메모리 사용량에 따라 정렬
  b : Batch 모드로 작동
  1 : CPU Core별로 사용량 보여줌
  
  [top -b -n 1 실행 결과](https://ucfbfc7788df9a3ed7117c485c95.dl.dropboxusercontent.com/cd/0/inline/B8Wxn3zrG9w1EzNcqe8GdXv3OPB62rAQYRGYI6-OP_0yaG3AplX4oXzNHgLPQSiOLVRey53e6nizK_9GZvhg5ZkLfWEYt1iW2FNMEKxGKiZe9s1ra_D4lrHYfyP73mM6jQ4oUX4hbM30ROsvOL2iRVM3LbKWhK0ohRCUCwbvFVOqzg/file#)
  
  3:58 : 3시간 58분 전에 서버가 구동
  load average : 현재 시스템이 얼마나 일을 하는지를 나타냄. 3개의 숫자는 1분, 5분, 15분 간의 평균 실행/대기 중인 프로세스의 수. CPU   코어수 보다 적으면 문제 없음
  Tasks : 프로세스 개수
  KiB Mem, Swap : 각 메모리의 사용량
  PR : 실행 우선순위
  VIRT, RES, SHR : 메모리 사용량 => 누수 check 가능
  S : 프로세스 상태(작업중, I/O 대기, 유휴 상태 등)

  VIRT, RES, SHR
  현재 프로세스가 사용하고 있는 메모리
  VIRT
  프로세스가 사용하고 있는 virtual memory의 전체 용량
  프로세스에 할당된 가상 메모리 전체
  SWAP + RES
  RES
  현재 프로세스가 사용하고 있는 물리 메모리의 양
  실제로 메모리에 올려서 사용하고 있는 물리 메모리
  실제로 메모리를 쓰고 있는 RES가 핵심!
  SHR
  다른 프로세스와 공유하고 있는 shared memory의 양
  예시로 라이브러리를 들 수 있음. 대부분의 리눅스 프로세스는 glibc라는 라이브러리를 참고하기에 이런 라이브러리를 공유 메모리에 올려서 사용
  Memory Commit
  프로세스가 커널에게 필요한 메모리를 요청하면 커널은 프로세스에 메모리 영역을 주고 실제로 할당은 하지 않지만 해당 영역을 프로세스에게 주었다는 것을 저장해둠
  이런 과정을 Memory commit이라 부름
  왜 커널은 프로세스의 메모리 요청에 따라 즉시 할당하지 않고 Memory Commit과 같은 기술을 사용해 요청을 지연시킬까?
  fork()와 같은 새로운 프로세스를 만들기 위한 콜을 처리해야 하기 때문
  fork() 시스템 콜을 사용하면 커널은 실행중인 프로세스와 똑같은 프로세스를 하나 더 만들고, exec() 시스템 콜을 통해 다른 프로세스로 변함. 이 때 확보한 메모리가 쓸모 없어질 수 있음
  COW(Copy-On-Write) 기법을 통해 복사된 메모리 영역에 실제 쓰기 작업이 발생한 후 실질적인 메모리 할당을 진행
  프로세스 상태
  참고글 : Load Average에 관하여
  SHR 옆에 있는 S 항목으로 볼 수 있음
  D : Uninterruptiable sleep. 디스크 혹은 네트워크 I/O를 대기
  R : 실행 중(CPU 자원을 소모)
  S : Sleeping 상태, 요청한 리소스를 즉시 사용 가능
  T : Traced or Stopped. 보통의 시스템에서 자주 볼 수 없는 상태
  Z : zombie. 부모 프로세스가 죽은 자식 프로세스
