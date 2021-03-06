# 1 Context
* 이 책에서는 GPL-2.0에 초점을 맞춘다. 
* 대부분의 compliance work은 시장에 배포하는 물리적 제품 혹은 download가능한 firmware에 초첨을 맞춘다. 
* 이 책에서는 물리적 device 내의 binary code에 GPL code가 있는지 확인한 후, 있을 경우 compliance 여부를 확인하기 위한 조치를 취하는 과정을 설명한다. 

# 2 Compliance Requirements
GPL은 몇가지 중요한 요구사항을 갖는다. 
1. 하나는 배포된 binary 또는 source code와 함께 license 사본을 제공하는 것이다. 
2. 다른 하나는 binary code를 stand-alone software application으로 배포하거나 물리적 제품의 일부로 배포할 때 source code에 access할 수 있게 해야 하는 것이다. 

GPL은 source code access 요구 사항을 준수하는 두가지 방법을 설명한다. 
1. "완전한 source code (complete coressponding source code)"를 제품에 동봉한다.(section 3a) 
2. "완전한 source code (complete coressponding source code)"를 제공하겠다는 서면 약정서 (written offer)를 포함시킨다. (section 3b)
* 이 두가지 방법은 source code가 전달되는 방법적인 부분은 다르지만, 완전한 source code (omplete coressponding source code)가 전달되어야 한다는 점에서 동일하다. 

# 3 Compliance Goals
GPL compliance 요구사항에 따른 compliance 목표는 다음과 같다. 
1. license 사본이 binary 혹은 source code 배포 시 동봉되는지 확인한다. 
2. 완전한 source code (omplete coressponding source code)가 제공되는지 확인한다. 

## 3.1 license 사본
* GPL code를 binary 또는 source code로 배포할 때, GPL license 사본이 동봉되어 있는지 확인해야 한다.
* GPL license는 물리적으로 또는 digital media로 제품에 포함시킬 수 있으며, 예는 다음과 같다. 
  * 스마트 TV의 사용자 매뉴얼 뒷면에 GPL license 사본이 물리적으로 인쇄되어 있음
  * 스마트폰 메뉴에서 Settings > Legal 또는 유사한 위치에 GPL license 사본이 있음

목표는 license가 쉽게 발견될 수 있도록 보장하는 것이다. 

## 3.2 완전한 source code (“Complete and Corresponding” Source Code)
* 이 부분이 GPL compliance의 어려운 부분이며, 이 책의 대부분이 이를 해결하려고 한다. 
* GPL software가 포함된 물리적 제품의 배포가 complinace engineer에게 가장 흔하고 문제가 되는 사용 사례이다. 
* 일반적인 예로는 device에 탑재되는 firmware 또는 website에서 다운로드 받을 수 있는 firmware update가 있다. 

### 3.2.1 두가지 주요 고려 사항
1. source code에 license 위반 사항이 포함되어 있지 않아야 한다. 
2. source code는 배포된 binary에 대해 완전(“complete and corresponding”)해야 한다. 

### 3.2.2 이상적인 상황
* 하나의 binary package 또는 firmware와 같은 binary package 모음이 있고, 이에 해당하는 source code archive(corresponding source code archive)가 있다. 
* corresponding source code archive는 open source component를 포함한다. 또한 LGPL software를 포함하는 binary를 relink하기 위한 일부 object file을 포함한다. 그리고, binary 또는 firmware를 rebuild하기 위한 가이드를 포함한다. 
* rebuild를 수행한 후, rebuild된 binary file과 원래의 binary은 정확하게 일치한다. 

### 3.2.3 실제 상황
* source code는 종종 완전(complete and corresponding)하지 않다. rebuild한 binary가 원본 대비 size가 다르거나, 완전하 다른 version이기도하다. 이는 GPL compliance를 보장하려고 할때 직면하는 가장 중요한 과제이다. 

# 4 Toolbox
도전 과제가 명확해졌으니, 이제 일을 해내기 위한 Tool에 대해 이야기하자. 아래에서 설명하는 Tool을 이용할 수 있으면, 이 가이드에 포함된 모든 것을 할 수 있다. 따라서, GPL compliance engineering 문제의 대부분을 해결할 수 있다. 

## 4.1 Default Tools
open source compliance engineer를 위한 기본 tool은 Linux이다. Fedora, CentOS, openSUSE, Debian 또는 Ubuntu와 같은 표준 Linux 배포판을 다운로드, 설치 및 설정해야한다. 이러한 배포판은 모두 우리 작업의 주요 역할을하는 기본 도구가 미리 설치되어 있다.(예: file, readelf, find, xargs, grep, dd, modinfo)

다음 위치에서 free installation을 받을 수 있다.
* Fedora : https://getfedora.org/
* openSUSE : http://opensuse.org/
* Debian : https://www.debian.org/
* Ubuntu : https://www.ubuntu.com/
* CentOS : https://www.centos.org/

compliance engineering을 위한 Armijn Hemel의 기본 installation은 Fedora이다. 그러나, 배포판을 선택할때는 engineer가 익숙한 것이 낫다. 

## 4.2 Binary Analysis
Binary의 분석에 도움이 되는 특정 tool들이 있다. 우리는 주로 Binary Analysis Tool (BAT)을 사용하지만, 편한것을 선택하여 사용하면 된다. 

### 4.2.1 Binary Analysis Tool (BAT)
Binary Analysis Tool을 사용하면 binary code를 들여다보고 compliance 문제를 쉽게 볼 수 있게 하여 FOSS를 배포 할 때 불확실성을 줄일 수 있다. 
BAT는 
* 30가지 종류 이상의 압축 파일, file system 및 media file을 열 수 있다; 
* Linux kernel과 BusyBox 문제를 찾을 수 있다;
* 동적 link된 library를 식별할 수 있다;
* source code로부터 추출한 정보를 갖고 있는 database를 이용하여 임의의 ELF, Android Dalvic, Java class file을 scan하고, 그 안에 어떤 software가 있는지 찾을 수 있다. 

BAT는 Apache license가 적용되어 누구든지 무료로 사용, 공부 및 개선 할 수 있다. 
* BAT download : https://github.com/armijnhemel/binaryanalysis
* BAT user guide : https://github.com/armijnhemel/binaryanalysis/blob/master/doc/bat-manual.pdf

### 4.2.2 binwalk
firmware를 분석하기 위한 또하나의 tool은 binwalk 이다. firmware image를 분석, reverse engineering 하는데 사용하기 쉽다. 
* binwalk download: https://github.com/devttys0/binwalk
* binwalk user guide: https://github.com/devttys0/binwalk/wiki

## 4.3 Source Code Analysis
binary code와 물리적 디바이스의 배포를 어떻게 다루는지에 대해 집중하고 있지만, 이것이 source code를 다루는 tool을 사용하지 않는다는 의미는 아니다. 실제 모든 우수한 compliance engineer는 source code license를 확인하는데 도움이 되는 tool을 하나 이상 보유하고 있다. 일반적으로 처음 시작하기에 좋은 tool은 FOSSology이다. 이는 무료 license scanner로써 source code archive를 검사하고 어떤 license가 있는지 알게해준다. 

### 4.3.1 FOSSology
FOSSology (https://www.fossology.org)는 compliance software system이자 toolkit이다.
* toolkit 측면 : command line에서 license, copyright scan을 실행할 수 있음
* system 측면 : compliance workflow 제공을 위한 database 및 web UI를 제공함

click 한번으로 software의 저작권 고지 및 SPDX file 혹은 README file을 생성할 수 있다. 

* 자세한 사항은 다음을 참조할 수 있다: https://www.fossology.org
* 간단한 "Get Started" 가이드는 다음을 참조할 수 있다: https://www.fossology.org/get-started

# 5 Binary file 분석
## 5.1 Binary라는 단어
이 책에서 "binary"라는 단어는 여러가지 의미를 갖을 수 있다. 
* single executable
* object file
* firmware
* 알수없는 data blob

이러한 유형의 binary 의미는 다음과 같은 공통점이 있다.
1. source code가 아님
2. open source code로부터 build 되어 만들어짐
3. 분석이 되어야함

## 5.2 Binary 분석을 위한 Tool
### 5.2.1 General Approach
Binary 분석은 여러가지 방법과 tool을 사용하여 수행할 수 있다. 일반적으로 Binary Analysis Tool (http://www.binaryanalysis.org)이나 binwalk (https://github.com/devttys0/binwalk)을 사용하여 binary code를 빠르고 간단하게 살펴볼 것을 권장한다.

### 5.2.2 Limitations
모든 binary가 unpack될 수 있는 것은 아니다. 예를들어 firmware가 난독화 / 암호화 되어있는 경우, unpack이 불가능할 수도 있다. 

### 5.2.3 Advanced Methods
난독화를 극복하기위한 Advanced Method에는 실행중인 장치에 cable을 연결하여 code를 가져오거나 network를 통해 침입하는 방법 등이 있을 수 있다. 이러한 기술들은 이 책에서는 다루지 않는다. 

# 6 (완전한 (“complete and corresponding”) 공개를 위한) Source Code 분석과 Rebuild
source code archive를 검사하여 다음 작업을 수행해야한다.
1. source code archive에서 문제가 될 수 있는 binary를 찾는다.
2. source code rebuild를 수행하여 original binary와 비교한다.
3. 옳지 않게 license된 code를 찾는다.

## 6.1 문제가 될 수 있는 binary 찾기
chipset 제조업체 및 ODM (Original Design Manufacturer)의 source code archive에는 종종 source code 외의 것이 포함되어 있다. 
* 이전 build에서 생성된 objece file
* binary 형식의 "out of tree" linux kernel module
* root file system과 같은 이전 build에서 생성된 library / executable
* file system image
* initial embedded ramdisk / initramfs file system와 함께 있는 linux kernel image
* 기타 firmware image

"file" 및 "xargs"와 함께 "find"를 사용하면 문제가 되는 file을 쉽게 찾을 수 있다. 모든 file에 대해 "file"을 실행하고 output을 result file에 redirect한 다음 그 result file을 원하는 방식으로 확인하면 된다. 다음은 시작하기 위한 command 예이다: 

`$ fnd /path/to/source/code -type f -print0 |
xargs --null fle > /path/to/result/fle`

여기서 다음 file을 찾아야 한다. 
* ELF file : architecture에 주의하라. ARM인 device에 예기치않게 MIPS같은 architecture의 executable이 포함될 수 있다. 이때는 이 binary를 제거해야한다. 
* PE32 and PE32+ fles : 이것들은 Windows binary이며, embedded Linux system과 관련된 source code release에는 보통 존재하지 않는다. (ActiveX plugin과 관련된 경우는 예외)
* Linux kernel boot images : 만약 이러한 file이 압축 file이나 U-Boot boot image의 일부로 존재하는 경우, 거의 항상 제거할 수 있다. (왜냐하면 이것들은 거의 확실하게 다른 configuration file을 이용하여 compile된 것이기 때문이다.) 이들을 포함시키는 것은 target device와 관계없는 licensing 요구사항이 잠재적으로 포함될 수 있는 것이다. 이러한 image들을 build process에 필요하다는 이유로 제거할 수 없다는 것은 source code가 완전하지 않음을 나타낸다. 찾기 위한 쉬운 방법은 "vmlinux", "vmlinuz" 또는 유사한 file을 search하는 것이다. 
* MacOS X fles : Windows file과 마찬가지로 embedded Linux system의 source code release에는 포함되지 않는다. 단, device가 Apple Mac OS X system에 제공할 software와 관련이 있는 경우는 예외이다. 찾기 위한 쉬운 방법은 "Mach-O"로 search하는 것이다. 이러한 file은 Google에서 배포하는 Android의 prebuilt toolchain source에 종종 존재한다. 

### 6.1.1 Object Files
object file (확장자 ".o")은 source code release에서 자주 발견된다. 이에 해당하는 ".c" 또는 ".cc" 파일이 존재한다면, "make clean"을 사용하여 object file을 제거하라. 만약, 해당하는 source code가 없다면 compliance 문제가 있을 수 있다. 

build process를 완전하게 하기 위해 object file이 필요하여 제거해서는 안되는 상황이 있을 수 있다. 한가지 예로는 LGPL licensed code와 정적 link하고 relink시 필요한 proprietary program의 object file이다.

### 6.1.2 “Out of tree” Linux Kernel Modules
일부 device는 default Linux kernel에서 지원하지 않는 component를 포함한다. 이들을 올바르게 작동하기 위해는 extra driver가 필요하며, 그러한 driver는 종종 Linux kernel module로 구현된다. 몇가지 예로는 WiFi driver, camera driver, 또는 firewalling module들이 있다. 이러한 extension은 두가지 공통적인 문제를 제공한다. 
1. 많은 driver는 쉬운 integration을 위해 vendor에 의해 pribuilt되어 제공된다. 이 driver의 license는 Linux kernel source의 license와 compatible한지 주의깊게 확인되어야 한다. 
2. extra driver source code를 공개할때, source code tree에 올바르게 통합되지 않을 수 있다. build system에 올바르게 interation하지 않으면 최종 전달 시 source code가 누락될 수 있다. 

### 6.1.3 Libraries/Executables
source code tree에는 종종 library 또는 executable binary가 있다. 이는 다음과 같은 경우에 발생한다: 
1. build 후 source code tree가 제대로 clean되지 않았다. 이는 source code에서 binary build후 "make clean"을 실행하지 않아서 종종 발생한다.
2. binary가 firmware를 build하기 위해 blueprint로 사용되는 "template" 또는 "skeleton" directory에 있다. template directory내의 directory 구조와 binary가 먼저 copy되고, build하는 동안 다른 file들이 추가된다.

이러한 binary들이 open source licensed code를 포함하고 배포를 위한 source code가 match되지 않는다면 제거해야 한다. 잘못된 version number, 다른 configuration, 또는 source code의 변경은 의도하지 않은 violation을 유발할 수 있다. 

이러한 문제를 고려할때의 중요한 질문은: "source code가 binary code에 대해 'complete and corresponding'하고, extra binary 또는 source code element는 없는가?" 이다. 

### 6.1.4 File System Images
전체 file system image가 source code tree에 포함되는 경우도 있다. 예를 들어, 많은 Android source code tree가 "boot.img", "system.img"같은 file system image를 포함하고 있다. 이 file에 coressponding source code 없이 open source가 포함되어 있다면 이는 원치 않는 위반이 발생할 수 있다.

### 6.1.5 Other Firmware Images
source code archive에 출시된 device와 관련이 없는 firmware가 발견된 경우도 있다. (다른 version의 software, package 등) 이러한 firmware는 원치 않는 불필요한 license 위반의 원인이 된다. 

## 6.2 Performing a Rebuild
source code가 'complete and corresponding'한지 확인하는 가장 효과적인 방법은 software를 rebuild하고 이를 original binary와 비교하는 것이다. (거의) 동일하다면 source code가 올바르다는 것을 나타낸다. 

### 6.2.1 Perfect(ish) Rebuilds
path와 timestamp 정보가 binary file에 포함되고 있는 경우, 정확한 rebuild를 하는 것은 상당히 어려울 수 있다. 따라서 "거의" 동일하다는 의미는 timestamp, filename path 및 이와 유사한 항목 정도만 차이가 있고 나머지는 동일해야함을 의미한다. 

### 6.2.2 Requirements
rebuild를 위해 다음 정보가 중요하다. 
1. build 환경에 대한 설명
2. full build 가이드

### 6.2.3 Goals
rebuild는 두가지 주요 목표가 있다. 
1. build가 동작하는지 확인
2. 결과 확인 (original binary와 동일한지)

### 6.2.4 build 환경 설명
rebuild된 binary와 원래의 binary를 올바르게 비교하기 위해서는 build 환경을 최대한 정확하게 설명해야 한다. 
이 설명에는 다음 내용이 포함되어야 한다. 
* 설치해야 하는 Linux 배포판 또는 OS의 이름 및 version (예: Fedora 7, 32 bit, or Ubuntu LTS 12.04, 64 bit).
* 추가로 설치해야할 package의 이름과 version
* 다음과 같은 default system에 수정이 필요한 사항: 
  * 필요한 symbolic link 생성
  * 필요한 directory 생성
  * 필요한 permission 변경
  * 존재햐야하는 file
  * 필요한 특정 user 생성
  * 필요한 환경 변수 설정

build 환경이 원래 환경과 다르면 (다른 compiler를 사용하거나, 혹은 compiler option이 다른것 조차도) 생성되는 code에 큰 영향을 줄 수 있다. 이는 binary file을 비교하여 source code가 원본 binary와 "complete and corresponding"하는지 확인하는 것을 어렵게 한다. 

### 6.2.5 Supplier/Client Roles
* 당신이 client에게 build 환경을 제공해야 하는 supplier라면, 최대한 자세하게 설명해야한다. 
* 당신이 build 환경 정보를 필요로하는 client라면, supplier에게 최대한 자세하게 요청해야 한다. 

### 6.2.6 Rebuild 가이드
build 가이드는 binary를 rebuild하는 정확한 단계를 명확하게 설명해야 한다. 여기에는 다음이 포함된다. 
* binary 또는 firmware를 rebuild하는데 필요한 정확한 command
* 예상되는 결과 (예: rebuild후 binary를 찾을 수 있는 위치)
이상적인 상황이라면, 가이드를 임의의 engineer에게 주었을때, 아무런 문제 없이 거의 완벽한 rebuild를 수행할 수 있어야 한다. 

### 6.2.7 Verifying the Instructions
rebuild 가이드는 test를 거쳐 완벽하게 되야 한다. clean한 system에서 test가 수행되는 것이 가장 좋다. rebuild 가이드에 작성되지 않은 system 수정 사항이 자주 존재할 수 있기 때문이다. project에 대해 지식이 없는 다른 engineer에게 rebuild를 수행해보게 하고, 발생한 모든 문제점을 문서화하라.

### 6.2.8 Verifying the Results
rebuild 후에는 결과를 확인해야 한다. 이전에 build된 것으로 확인하지 않도록 주의하라. 이 작업을 수행하는 두가지 방법은 다음과 같다. 
1. binary checksum
2. binary 내용

#### 6.2.8.1 The Checksum of the Binaries
Rebuild된 file이 original binary와 동일한 hash를 갖는 경우 file은 동일한 것이다. 이를 위한 도구로 MD5 hash의 경우 "md5sum", SHA256 hash의 경우 "sha256sum"이 있다. 다음 명령은 두개의 binary에 대해 hash를 계산하고 결과를 출력한다. 

`$ md5sum /path/to/original/binary /path/to/new/binary`

`$ sha256sum /path/to/original/binary /path/to/new/binary`

이러한 명령어는 firmware내 개별 binary(예: "smbd" 또는 "iptables")에 대해 실행하는 것이 좋으며, build된 전체 firmware에 대해서는 실행하지 않는 것이 좋다. 이는 firmware의 checksum이 결코 같지 않을 수 있기 때문이다. BusyBox나 Linux kernel과 같은 일부 binary는 매번 다른 checksum을 반환한다. 이는 기본적으로 내부에 timestamp가 포함되어 있기 때문이다. 

#### 6.2.8.2 The Content of the Binaries

많은 경우, Original binary와 rebuild된 binary의 checksum이 동일하지 않다. 이는 source code file의 path와 timestamp가 포함되어 있기 때문이다. 환경을 신중하게 설정하더라도 각 build마다 달라질 수 있다. 하지만, 다음 단계를 통해 rebuild된 binary가 original binary에 충분히 비슷한지를 확인할 수 있다. 

1. File 크기 확인
2. File 내용 비교

##### 6.2.8.2.1 File 크기 확인
Rebuild된 binary의 file 크기는 original binary와 매우 비슷해야한다. 크게 차이가 나는 경우, 먼저 하나의 binary만 "stripped"(debugging symbol 제거)된 것인지 확인하라. 만약 그렇다면, "strip" 명령을 사용하여 다른 binary도 제거한다. (이 tool은 toolchain에 포함되어 있다.) 여전히 file size가 차이난다면, 두 binary는 같지 않을 가능성이 크다. 

##### 6.2.8.2.2 File 내용 비교
"strings" 명령은 binary로부터 사람이 읽을 수 있는 문자열을 추출하는데 사용할 수 있다. 다음의 간단한 3단계 process를 통해 file의 내용을 비교할 수 있다. 

1. binary의 rebuild
2. "strings"를 이용하여 내용 추출
3. 결과를 original binary와 비교

다음 command가 file의 내용을 추출하게 해준다. 

`$ strings /path/to/old/binary > /tmp/strings.old`

`$ strings /path/to/new/binary > /tmp/strings.new`

`$ diff -u /tmp/strings.old /tmp/strings.new | less`

동일하지 않은 부분이 단지 timestamp와 path name이라면 두 binary는 실제로 동일하다고 거의 볼 수 있다. 

## 6.3 Finding Incorrectly Licensed Code
Open source licensed program을 수정하면서 license가 올바르게 부여되지 않을 수 있다. 한가지 고전적인 예는 chipset 제조사에 의해 수정이된 Linux kernel에 license 선언문이 누락되거나 또는 GPL과 호환되지 않는 license 선언문을 포함하는 경우이다. chipset 제조사가 추가한 Linux kernel driver code를 relicensing하는 것과 관련한 enforcement case가 있었다. 이러한 상황을 피하려면 다음 단계를 수행해야 한다: 

1. 잘못 license된 file을 찾으라. 
2. 누가 잘못 license된 file을 제공했는지 확인하라. 
3. 해당 file이 실제로 필요한지 확인하라. 
4. 허용되는 license하의 version을 찾으라. 
5. license 변경 권한을 요청하라.
6. software를 재작성하라. 
 
### 6.3.1 잘못 license된 file을 찾으라
License scanner를 사용하면 source code file의 license를 찾을 수 있다. 많은 license scanner가 있으며, 이 중 일부는 proprietary이고(예: Black Duck Protex, Palamida, Whitesource, Protecode, FOSSID, FOSSA), 다른 것들은 open source이다 (FOSSology, Scancode). license scanning은 올바르게 수행하기가 어렵고, 비표준 license header를 사용하거나, license text가 전혀 없는 source file도 많기 때문에 어떤 license scanner도 perfect job을 수행하지는 못한다. 

직접 작성한 code의 경우, license scanner가 license를 쉽게 식별하게 할 수 있는 방법이 있다. 한가지 예는 SPDX short identifier를 사용하는 것이다. SPDX는 package content를 설명하는 간단하고 표준화된 방법이고 license 정보 관리를 위해 업계 전반에 적용되고 있다. SPDX에 대한 자세한 내용은 https://spdx.org/에서 확인할 수 있다. 

### 6.3.2 누가 잘못 license된 file을 제공했는지 확인하라
잘못 license된 file을 발견한 후에는 누가 그것을 제공했는지를 이해하는 것이 중요하다. 이는 보통 두가지 원인 중 하나로 귀결된다.

1. Upstream project
2. ODM / Chipset 제조사

만약 당신이 open source code가 올바른 license 선언문이 없는 상황을 확인하여 upstream project에 이를 알린다면, 그들은 감사해할 것이다. Source에서 issue를 고치는 것은 모두에게 보다 나은 정보를 제공한다. Linux kernel의 경우 정확한 license 정보가 없는 file이 많이 있음에 주의하라. 그러나, 수년동안 Linux kernel에 존재하는 기존의 file은 문제로 간주되지 않는다. 알려진 file을 신속하게 filtering하는 방법은 https://lwn.net/Articles/552758에 설명되어 있다. 

때때로 회사는 "trusted" upstream project 목록을 가지고 있다. 그들은 그 project로부터 가져온 모든 code는 더 이상의 검토 없이 사용할 수 있다고 간주한다. "trusted" project는 무엇이며, 그 이유는 무엇인지에 대한 결정은 사람 또는 조직마다 달라질 수 있다. 예를 든다면, kernel.org에서 직접 얻은 Linux kernel은 신뢰할 수 있지만, GitHub의 임의 kernel fork는 trust할 수 없다는 것이 일반적이다. 

### 6.3.3 해당 file이 실제로 필요한지 확인하라
문제가 되는 file이 upstream project에 의해 제공되는 것이 아닌 경우, file을 제거하는 것이 좋다. 예를 들어, Microsoft Windows driver용 file이 Linux kernel source code tree에 존재한다면, 안전하게 제거할 수 있다. 

마찬가지로 최종 binary 또는 build process에 사용되지 않는 파일이라면 안전하게 제거할 수 있다. 안전하게 제거할 수 있는 file인지 확인하는 한 방법은 file을 source code tree에서 제거하고 binary를 다시 build하는 것이다. (이전 build의 build 산출물을 완전히 제거하고, full build하는 방식으로.) 결과 binary가 동일하면 (또는 충분히 유사하면) file을 안전하게 제거할 수 있는 것이다.

### 6.3.4 허용되는 license하의 version을 찾으라
과거 Embedded Linux 산업에서는 (특히 driver) 잘못 license된 source code file과 관련하여 많은 실수가 있었다. 일부 vendor는 허용 가능한 license하의 새로운 driver version으로 이미 relicense했지만, ODM들은 update에 대해 알지 못하거나 새로운 driver가 자신들의 component와 제대로 동작하는지 test하는 것을 원하지 않기 때문에 여전히 old driver version을 제공하고 있다. 

### 6.3.5 License 변경 권한을 요청하라
License를 변경하는 것은 일반적으로 가장 어려운 solution이지만 때로는 software를 재작성하는 것 외의 유일한 option일 수 있다. file이 사용되었지만 올바른 license가 없는 경우, copyright owner에게 file에 대해 relicense를 요청해야 한다. 일부 copyright owner는 해주지 않을 수도 있지만, 다른 제조사나 개발자들은 반대하지 않을 수도 있다.

