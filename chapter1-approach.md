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
* source code는 종종 완전 (complete and corresponding)하지 않다. binary를 rebuild할 때, 원본과 비교하여 size가 다르거나, 완전하 다른 version이다. 이는 GPL compliance를 보장하려고 할때 직면하는 가장 중요한 과제이다. 
