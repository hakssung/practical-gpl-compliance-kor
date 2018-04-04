Chapter2. Common Pitfalls
=========================

Pitfall \#1: Toolchain
======================

-   rebuild를 수행하는데 필수적인 요소 중 하나는 toolchain이다. 이는
    compiler, assemble/linker 등과 C library로 구성된다. 

-   embedded linux system의 경우, 

    -   compiler는 거의 항상 GCC이고(LLVM이 사용되기 시작함), 

    -   assembler/linker는 GNU binutils에서 제공되며, 

    -   C library는 

        -   일반적인 embedded Linux에서는 glibc 혹은 uClibc (둘다 LGPL
            license), 혹은 musl (MIT license)이고, 

        -   Android system에서는 bionic이다. 

<!-- -->

-   toolchain은 종종 compliant하지 않은 것으로 판명된다. 

    -   일반적인 시나리오는 GCC와 GNU binutils를 가진 toolchain이 source
        code나 source code에 대한 offer없이 binary-only form으로
        제공된다는 것이다. 

        -   제공된 binary toolchain을 사용하여 binary를 rebuild할 수도
            있지만, 이는 올바른 방법은 아니다. 

        -   GCC compiler와 GNU binutils은 version에 따라 GPL-2.0 또는
            GPL-3.0하에 배포된다. 

        -   source code 또는 written offer가 binary와 함께 제공되어야
            한다. 

    -   glibc 또는 uClibc를 사용할때, 추가적인 reason이 있다. 

        -   (prebuilt된) (C library) toolchain의 부분이 firmware image로
            복사되는 경우가 있다. 이는 C library를 rebuild하기 위한
            source code 및 configuration도 역시 (LGPL license 조건에
            따라) 제공되어야 함을 의미한다.

        -   이러한 요구사항을 충족시키는 가장 빠른 방법은 완전한
            toolchain source code를 확보하는 것이다.
