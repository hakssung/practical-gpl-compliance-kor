# Context
* 이 책에서는 GPL-2.0에 초점을 맞춘다. 
* 대부분의 compliance work은 시장에 보내지는 물리적 제품 혹은 firmware download에 초점을 맞춘다. 
* 이 책에서는 물리적 device 내의 binary code에 GPL code가 있는지 확인한 후, 있을 경우 compliance 여부를 확인하기 위한 조치를 취하는 과정을 설명한다. 
# Compliance Requirements
GPL은 몇가지 중요한 요구사항을 갖는다. 
1. 하나는 배포된 binary 또는 source code와 함께 license 사본을 제공하는 것이다. 
2. 다른 하나는 binary code를 stand-alone software application으로 배포하거나 물리적 제품의 일부로 배포할 때 source code에 access할 수 있게 해야 하는 것이다. 

GPL은 source code access 요구 사항을 준수하는 두가지 방법을 설명한다. 
1. "완전한 source code (complete coressponding source code)"를 제품에 동봉한다.(section 3a) 
2. "완전한 source code (complete coressponding source code)"를 제공하겠다는 서면 약정서 (written offer)를 포함시킨다. (section 3b)
* 이 두가지 방법은 source code가 전달되는 방법적인 부분은 다르지만, 완전한 source code (omplete coressponding source code)가 전달되어야 한다는 점은 다르지 않다. 

# Compliance Goals
GPL compliance 요구사항에 따른 compliance 목표는 다음과 같다. 
1. license 사본이 binary 혹은 source code 배포 시 동봉되는지 확인한다. 
2. 완전한 source code (omplete coressponding source code)가 제공되는지 확인한다. 

## license 사본
* GPL code를 binary 또는 source code로 배포할 때, GPL license 사본이 동봉되어 있는지 확인해야 한다.
* GPL license는 물리적으로 또는 digital media로 제품에 포함시킬 수 있으며, 예는 다음과 같다. 
  * 스마트 TV의 사용자 매뉴얼 뒷면에 GPL license 사본이 물리적으로 인쇄되어 있음
  * 스마트폰 메뉴에서 Settings > Legal 또는 유사한 위치에 GPL license 사본이 있음

목표는 license가 쉽게 발견될 수 있도록 보장하는 것이다. 

## 완전한 source code (“Complete and Corresponding” Source Code)
* 이 부분이 GPL compliance의 어려운 부분이며, 이 책의 대부분이 이를 해결하려고 한다. 
* GPL software가 포함된 물리적 제품의 배포가 complinace engineer에게 가장 흔하고 문제가 되는 사용 사례이다. 
* 일반적인 예로는 device에 탑재되는 firmware 또는 website에서 다운로드 받을 수 있는 firmware update가 있다. 

### 두가지 주요 고려 사항
1. source code에 license 위반 사항이 포함되어 있지 않아야 한다. 
2. source code는 배포된 binary에 대해 완전(“complete and corresponding”)해야 한다. 

### 이상적인 상황
* 하나의 binary package 또는 firmware와 같은 binary package 모음이 있고, 이에 해당하는 source code archive(corresponding source code archive)가 있다. 
* corresponding source code archive는 open source component를 포함한다. 또한 LGPL software를 포함하는 binary를 relink하기 위한 일부 object file을 포함한다. 그리고, binary 또는 firmware를 rebuild하기 위한 가이드를 포함한다. 
* rebuild를 수행한 후, rebuild된 binary file과 원래의 binary은 정확하게 일치한다. 

### 실제 상황
* source code는 종종 완전 (complete and corresponding)하지 않다. rebuild한 binary가 원본 대비 size가 다르거나, 완전하 다른 version이기도한다. 이는 GPL compliance를 보장하려고 할때 직면하는 가장 중요한 과제이다. 

# Toolbox
도전 과제가 명확해졌으니, 이제 일을 해내기 위한 Tool에 대해 이야기하자. 아래에서 설명하는 Tool을 이용할 수 있으면, 이 가이드에 포함된 모든 것을 할 수 있다. 따라서, GPL compliance engineering 문제의 대부분을 해결할 수 있다. 

## Default Tools
open source compliance engineer를 위한 기본 tool은 Linux이다. Fedora, CentOS, openSUSE, Debian 또는 Ubuntu와 같은 표준 Linux 배포판을 다운로드, 설치 및 설정해야한다. 이러한 배포판은 모두 우리 작업의 주요 역할을하는 기본 도구가 미리 설치되어 있다.(예: file, readelf, find, xargs, grep, dd, modinfo)

다음 위치에서 free installation을 받을 수 있다.
* Fedora : https://getfedora.org/
* openSUSE : http://opensuse.org/
* Debian : https://www.debian.org/
* Ubuntu : https://www.ubuntu.com/
* CentOS : https://www.centos.org/

compliance engineering을 위한 Armijn Hemel의 기본 installation은 Fedora이다. 그러나, 배포판을 선택할때는 engineer가 익숙한 것이 낫다. 

## Binary Analysis
Binary의 분석에 도움이 되는 특정 tool들이 있다. 우리는 주로 Binary Analysis Tool (BAT)을 사용하지만, 편한것을 선택하여 사용하면 된다. 

### Binary Analysis Tool (BAT)
Binary Analysis Tool을 사용하면 binary code를 들여다보고 compliance 문제를 쉽게 볼 수 있게 하여 FOSS를 배포 할 때 불확실성을 줄일 수 있다. 
BAT는 
* 30가지 종류 이상의 압축 파일, file system 및 media file을 열 수 있다; 
* Linux kernel과 BusyBox 문제를 찾을 수 있다;
* 동적 link된 library를 식별할 수 있다;
* source code로부터 추출한 정보를 갖고 있는 database를 이용하여 임의의 ELF, Android Dalvic, Java class file을 scan하고, 그 안에 어떤 software가 있는지 찾을 수 있다. 

BAT는 Apache license가 적용되어 누구든지 무료로 사용, 공부 및 개선 할 수 있다. 
* BAT download : https://github.com/armijnhemel/binaryanalysis
* BAT user guide : https://github.com/armijnhemel/binaryanalysis/blob/master/doc/bat-manual.pdf

### binwalk
firmware를 분석하기 위한 또하나의 tool은 binwalk 이다. firmware image를 분석, reverse engineering 하는데 사용하기 쉽다. 
* binwalk download: https://github.com/devttys0/binwalk
* binwalk user guide: https://github.com/devttys0/binwalk/wiki

## Source Code Analysis
binary code와 물리적 디바이스의 배포를 어떻게 다루는지에 대해 집중하지만, 이것이 source code를 다루는 tool을 사용하지 않는다는 의미는 아니다. 실제 모든 우수한 compliance engineer는 source code license를 확인하는데 도움이 되는 tool을 하나 이상 보유하고 있다. 시작하기에 좋은 것은  일반적으로 FOSSology이다. 이는 무료 license scanner로써 source code archive를 검사하고 어떤 license가 있는지 알게해준다. 

### FOSSology
