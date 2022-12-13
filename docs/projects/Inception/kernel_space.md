---
layout: default
title: kernel space
nav_order: 3
grand_parent: Projects
parent: Inception
permalink: /docs/projects/Inception/kernel_space
---

* [Kernel Space](#kernel-space)
	* [chroot](#chroot)
	* [Linux namespace](#linux-namespace)
	* [namespace API](#namespace-api)
		* [clone](#clone)
		* [unshare](#unshare)
		* [setns](#setns)
		* [proc](#proc)
	* [namespace](#namespace)
		* [mnt](#mnt)
		* [uts](#uts)
		* [ipc](#ipc)
		* [pid](#pid)
		* [net](#net)
		* [user](#user)
		* [cgroup](#cgroup)


# Kernel Space

## chroot

대부분의 모든 UNIX 운영 체제는 현재 실행 중인 프로세스의 루트 디렉터리를 변경할 수 있다. UNIX 버전 7에서 chroot가 처음 등장한 것에서 비롯되었으며, Linux에서는 시스템 호출 또는 해당 독립 실행형 래퍼 프로그램으로 chroot를 사용할 수 있다. chroot는 1991년에 어떤 사람이 보안 해커를 감시하기 위해 허니팟으로 사용했기 때문에 "감옥"이라고도 한다.  

```bash
> mkdir -p new-root/{bin,lib64}
> cp /bin/bash new-root/bin
> cp /lib64/{ld-linux-x86-64.so*,libc.so*,libdl.so.2,libreadline.so*,libtinfo.so*} new-root/lib64
> sudo chroot new-root
```

자체 chroot 환경을 실행하려면 새로운 루트 디렉터리를 생성하고, bash 쉘과 해당 의존 항목을 복사하고 chroot를 실행한다. 빌트인 함수만을 가지고 있기 때문에 아직 쓸모가 없는 bash이다.  

현재 작업 디렉토리는 chroot가 호출된 때에서 변경되지 않지만, 상대 경록는 여전히 새 루트 외부의 파일을 참조할 수 있다. 이 호출이 루트 경로만 변경하고 다른 것은 변경하지 않기 때문이다. 그래서 루트 사용자는 다음과 같은 프로그램을 실행하여 감옥에서 쉽게 탈출할 수 있다.  

```c
#include <sys/stat.h> 
#include <unistd.h> 

int main(void) 
{ 
    mkdir(".out", 0755); 
    chroot(".out"); 
    chdir("../../../../../"); 
    chroot("."); 
    return execl("/bin/bash", "-i", NULL); 
}
```

현재 감옥을 덮어써서 새로운 감옥을 만들고 작업을 chroot 외부 환경에 상대 경로로 직접 변경한다. chroot 호출은 다른 bash 쉘을 생성하여 감옥 외부로 이동된다.  

그렇기 때문에 유용한 감옥을 사용하려면 적절한 루트 파일 시스템이 필요하다. 여기에는 모든 바이너리, 라이브러리 및 필요한 파일 구조가 포함된다. 다음 코드는 루트 파일 시스템을 skopeo와 umoci를 사용하여 가져온다.  

```bash
> skopeo copy docker://opensuse/tumbleweed:latest oci:tumbleweed:latest 
[output removed] 
> sudo umoci unpack --image tumbleweed:latest bundle 
[output removed]
```

새로 다운로드하고 추출한 rootfs에 chroot를 사용하여 감옥을 설정할 수 있다.  

```bash
> sudo chroot 번들/rootfs 
#
```

그러나 아직 프로세스 관점에서 감옥을 외부에서 몰래 들여다볼 수 있다. 심지어 감옥에서 실행되는 프로그램을 외부에서 죽일 수도 있다. 

```bash
> mkdir /proc 
> mount -t proc proc /proc 
> ps aux 
[출력 제거됨]
```

네트워크 격리도 없다. 감옥에 누락된 격리는 많은 보안 관련 문제로 이어진다. 

```bash
> mkdir /sys 
> 마운트 -t sysfs sys /sys 
> ls /sys/class/net 
eth0 lo
```

이 문제를 해결하기 위해 Linux namespace가 사용된다.  

## Linux namespace

namespace는 2002년 Linux2.4.19와 함께 도입된 커널 기능이다. namespace는 추상화 계층에서 특정 글로벌 시스템 리소스를 래핑한다. 이는 namespace 내의 프로세스가 자체적으로 격리된 리소스 인스턴스를 갖는 것처럼 보인다. 커널 namespace 추상화를 통해 서로 다른 프로세스 그룹이 시스템에 대해 서로 다른 뷰를 가지게 된다.  

## namespace API

namespace API는 세가지 주요 시스템 호출로 구성된다. 

### clone

clone API 함수는 fork와 비슷하게 새로운 자식 프로세스를 생성한다. 그러나 fork와 다르게 clone API는 자식 프로세스에게 메모리 공간, 파일 디스크립트 테이블, 시그널 핸들러 같은 실행 컨텍스트의 일부를 호출 프로세스와 공유한다. 그리고 다른 namespace 플레그를 넘겨주어 자식 프로세스에 새로운 namespace를 생성할 수 있다.  

### unshare

unshare 함수는 현재 프로세스가 다른 프로세스와 공유 중인 실행 컨텍스트의 일부를 연결 해제할 수 있다.  

### setns

setns 함수는 호출 thread를 제공된 namespace 파일 디스크립터와 다시 연결한다. 기존 namespace를 합치는데 사용할 수 있다.  

### proc

proc 파일 시스템은 추가 namespace 관련 파일을 채운다. Linux3.8 이후로, /proc/$PID/ns 파일은 "매직" 링크이다. 참조된 namespacee에 대한 작업을 수행학기 위한 핸들로 사용할 수 있다.  

```bash
> ls -Gg /proc/self/ns/
total 0
lrwxrwxrwx 1 0 Feb  6 18:32 cgroup -> 'cgroup:[4026531835]'
lrwxrwxrwx 1 0 Feb  6 18:32 ipc -> 'ipc:[4026531839]'
lrwxrwxrwx 1 0 Feb  6 18:32 mnt -> 'mnt:[4026531840]'
lrwxrwxrwx 1 0 Feb  6 18:32 net -> 'net:[4026532008]'
lrwxrwxrwx 1 0 Feb  6 18:32 pid -> 'pid:[4026531836]'
lrwxrwxrwx 1 0 Feb  6 18:32 pid_for_children -> 'pid:[4026531836]'
lrwxrwxrwx 1 0 Feb  6 18:32 user -> 'user:[4026531837]'
lrwxrwxrwx 1 0 Feb  6 18:32 uts -> 'uts:[4026531838]'
```

이를 통해 특정 프로세스가 상주하는 namespace를 추적할 수 있다. 여기에는 시스템 호출에 대한 전용 래퍼 프로그램이 포함되어 있다. 현재 액세스 가능한 모든 namespace 또는 주어진 단일 namespace에 대한 유용한 정보를 나열한다.  

## namespace

### mnt

mnt namespace를 사용하여 Linux는 일련의 마운트 지점을 프로세스 그룹별로 분리할 수 있다. 감옥과 유사하지만 더 안전한 방식으로 환경을 만들 수 있다. 다음과 같이 API 시스템 호출이나 unshare 명령 라인 툴로 쉽게 수행할 수 있다.  

```bash
> sudo unshare -m
# mkdir mount-dir
# mount -n -o size=10m -t tmpfs tmpfs mount-dir
# df mount-dir
Filesystem     1K-blocks  Used Available Use% Mounted on
tmpfs              10240     0     10240   0% <PATH>/mount-dir
# touch mount-dir/{0,1,2}
```

```bash
> ls mount-dir
> grep mount-dir /proc/mounts
>
```

호스트 시스템 레벨에서 사용할 수 없는 tmpfs이 성공적으로 마운트된 것을 볼 수 있다.  

마운트 지점에 사용되는 실제 메모리는 가상 파일 시스템(VFS)이라는 추상화 계층에 있다. 이 계층은 커널의 일부이며 다른 모든 파일 시스템의 기반이 된다. namespace가 파괴되면 마운트 메모리는 복구할 수 없게 손실된다. mount namespace 추상화는 루트 권한 없이도 루트 사용자인 전체 가상 환경을 생성할 수 있는 가능성을 제공한다.  

호스트 시스템에서 proc 파일 시스템 내부의 mountinfo 파일을 통해 마운트 지점을 볼 수 있다.  

```bash
> grep mount-dir /proc/$(pgrep -u root bash)/mountinfo
349 399 0:84 / /mount-dir rw,relatime - tmpfs tmpfs rw,size=1024k
```

프로그램은 사용된 namespace를 참조하는 해당 /proc/$PID/ns/mntt 파일에 파일 핸들을 유지하는 경향이 있다. mount namespace 관련 구현 시나리오는 복잡할 수 있지만 유연한 컨테이너 파일 시스템 트리를 생성할 수 있는 기능을 제공한다. 마운트는 다양한 특징을 가질 수 있다. 이는 Linux 커널의 [공유 하위 트리 문서](https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt)에서 잘 설명된다. 

### uts

uts namespace는 현재 호스트 시스템에서 도메인 및 호스트 이름의 공유를 해제할 수 있다. 다음과 같이 사용한다.  

```bash
> sudo unshare -u
# hostname
nb
# hostname new-hostname
# hostname
new-hostname
```

```bash
> hostname
nb
```

시스템 레벨에서는 아무것도 변경되지 않았다. uts namespace는 특히 컨테이너 네트워킹과 관련하여 컨테이너화의 추가 기능이다. 

### ipc

ipc namespace는 프로세스 간 통신 리소스를 격리한다. 특히 System V IPC 객체와 POSIX 메시지 대기열이다. 한 가지 예시로 오용을 방지하기 위해 두 프로세스 간의 공유 메모리(SHM)를 분리한다. 대신에 각 프로세스는 공유 메모리 세그먼트에 대해 동일한 식별자를 사용하고 두 개의 개별 영역을 생성할 수 있다. ipc namespace가 소멸되면 namespace의 ipc 객체도 자동으로 소멸된다.  

### pid

pid namespace는 프로세스에 독립적인 프로세스 식별자(PID) 세트를 제공한다. 즉, 서로 다른 namespace에 있는 프로세스가 동일한 PID를 소유할 수 있다. 결국 프로세스에는 두가지 PID, namespace 내부의 PID와 호스트 시스템의 PID가 있다. pid namespace는 중첩될 수 있으므로 새 프로세스가 생성되면 현재 namespace에서 초기 PID namespace까지 각 namespace에 대한 PID를 가지게 된다.  

PID namespace에서 생성된 첫 번째 프로세스는 숫자 1을 얻고 일반 초기화 프로세스와 동일한 특수 처리를 모두 얻는다. 예를 들어 namespace 내의 모든 프로세스는 호스트 PID 1이 아닌 namespace의 PID 1로 부모가 변경된다. 또한 이 프로세스를 종료하면 PID namespace의 모든 프로세스와 모든 하위 프로세스가 즉시 종료된다. 다음은 새로운 PID namespace를 만든다.  

```bash
> sudo unshare -fp --mount-proc
# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.4  0.6  18688  6608 pts/0    S    23:15   0:00 -bash
root        39  0.0  0.1  35480  1768 pts/0    R+   23:15   0:00 ps aux
```

잘 분리된 것을 볼 수 있다. 새 namespace에서 proc 파일 시스템을 다시 마운트하려면 --mount-proc 플래그가 필요하다. 그렇지 않으면 namespace에 해당하는 PID 하위 트리를 볼 수 없다. 

### net

net namespace는 네트워크 스택을 가상화하는데 사용할 수 있다. 각 네트워크 namespace는 /proc/net에 자체 리소스 속성을 포함한다. 또한 네트워크 namespace는 초기 생성 시 루프백 인터페이스만 포함한다.  

```bash
> sudo unshare -n
# ip l
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

모든 네트워크 인터페이스(물리적 또는 가상)는 namespace당 정확히 한 번만 존재한다. namespace 간에 인터페이스를 이동할 수 있다. 각 namespace에는 비공개 IP 주소 집합, 자체 라우팅 테이블, 소켓 목록, 연결 추적 테이블, 방화벽 및 기타 네트워크 관련 리소스가 포함되어 있다.  

네트워크 namespace를 제거하면 모든 가상이 제거되고 그 안에 있는 모든 물리적 인터페이스가 다시 초기 네트워크 namespace로 이동된다.  

net namespace의 예시로는 가상 이더넷(veth) 인터페이스 쌍을 통해 소프트웨어 정의 네트워크(SDN)를 생성하는 것이다. 네트워크 쌍의 한 쪽은 브리지 인터페이스에 연결되고, 다른 쪽은 대상 컨테이너에 할당된다.  

### user

user namespace는 사용자 및 그룹 ID를 격리한다. Linux 3.8에서는 실제로 권한이 없어도 user namespace를 생성할 수 있다. user namespace를 사용하면 프로세스의 사용자 및 그룹 ID가 namespace 외부와 다를 수 있다.  

namespace 생성 후 /proc/$PID/{u, g}id_map 파일은 PID의 사용자 및 그룹 ID에 대한 매핑을 노출한다. 일반적으로 이러한 파일 내의 각 중에는 두 사용자 namespacee 간의 연속적인 사용자 ID 범위에 대한 일대일 매핑이 포함되며 다음과 같이 표시될 수 있다.  

```bash
> cat /proc/$PID/uid_map
0 1000 1
```

시작 사용자 ID가 0인 namespace는 ID 1000에서 시작하는 범위에 매핑된다. 정의된 길이가 1이므로 ID가 1000인 사용자에게만 적용된다.  

이제 프로세스가 파일에 액세스하려고 하면 권한 확인을 위해 해당 사용자 및 그룹 ID가 초기 사용자 namespace에 매핑된다. 프로세스가 파일 사용자 및 그룹 ID를 검색할 때(stat(2)를 통해) ID는 반대 방향으로 매핑된다.  

/proc/$PID/setgroups 파일에는 user namespace 내에서 setgroups을 시스템 호출할 수 있는 권한을 활성화 또는 비활성화하는 허용 또는 거부가 포함되어 있다. 이 파일은 user namespace에 도입된 추가 보안 문제를 해결하기 위해 추가되었다. 권한이 없는 프로세스가 사용자에게 모든 권한이 있는 새 namespace를 생성할 수 있다. 이전에 권한이 없었던 이 사용자는 이전에 가지고 있지 않았던 파일에 접근하기 위해 setgroups를 통해 그룹을 삭제할 수 있다.  

### cgroup

cgroup namespace는 리소스 제한, 우선순위 지정 및 제어를 지원한다. 호스트 정보가 namespace로 유출되는 것을 방지하기 위해 추가되었다. 기본적으로 커널은 /sys/fs/cgroup에 cgroup을 노출한다. 새 cgroup을 생성하려면 해당 위치에 새 하위 디렉터리를 생성하면 된다.  

```bash
> sudo mkdir /sys/fs/cgroup/memory/demo
> ls /sys/fs/cgroup/memory/demo
cgroup.clone_children
cgroup.event_control
cgroup.procs
memory.failcnt
memory.force_empty
memory.kmem.failcnt
memory.kmem.limit_in_bytes
memory.kmem.max_usage_in_bytes
memory.kmem.slabinfo
memory.kmem.tcp.failcnt
memory.kmem.tcp.limit_in_bytes
memory.kmem.tcp.max_usage_in_bytes
memory.kmem.tcp.usage_in_bytes
memory.kmem.usage_in_bytes
memory.limit_in_bytes
memory.max_usage_in_bytes
memory.move_charge_at_immigrate
memory.numa_stat
memory.oom_control
memory.pressure_level
memory.soft_limit_in_bytes
memory.stat
memory.swappiness
memory.usage_in_bytes
memory.use_hierarchy
notify_on_release
tasks
```

이미 몇가지 기본값이 있음을 알 수 있다. 여기에서 해당 cgroup에 대한 메모리 제한을 설정할 수 있다.  

```bash
> sudo su
# echo 100000000 > /sys/fs/cgroup/memory/demo/memory.limit_in_bytes
# echo 0 > /sys/fs/cgroup/memory/demo/memory.swappiness
```

cgroup에 프로세스를 지정하기 위해 cgroup.procs 파일에 해당 PID를 작성할 수 있다.  

```bash
# echo $$ > /sys/fs/cgroup/memory/demo/cgroup.procs
```

이제 애플리케이션을 실행하여 허용된 100MB 이상의 메모리를 사용해볼 수 있다.  



참조 : https://docs.docker.com/get-started/, https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504  
