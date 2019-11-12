# TargetExpert

 TargetExpert란 Target Machine의 Firmware를 각각의 Target Machine별로 만들어야 하는 불편함을 제거하기 위한 프로젝트이다.
총 3단계 LLL, MLL, HLL로 이루어져있다. LLL에서 거의 공통된 Function 또는 Class Name을 가진 API로 각각의 Target Mechine을
별도로 구성한 후, MLL로 완전히 공통된 Function 또는 Class Name으로 통합하여 아두이노와 같은 기능을 할 수 있도록 API화 시킨
인베디드용 Framewokr이다.

#### 간단한 실습

아직 윈도우os에서는 실행이 힘이 들어 가상머신을 이용해 우분투 리눅스에서 실습하였다. 

우분투 리누스에 툴 체인을 위한 아두이노를 설치힌다. 후에 CLION을 설치한다.
아두이노 안에 있는 툴 체인을 사용 하기 위해 링크를 걸고 TESuit를 넣는다.
터미널에서 

<pre><code>
sudo chmod a+re /dev/ttyACMO
 ln -s ../../TESuit/
ln -s ../../TargetExpert/
</code></pre>

 입력해준다.

후에 CLION에서
main.c 생성 후 
<pre><code>

 #include "Common.inc.h"
 #include <util/delay.h>
 
 bool_Blink = false;
 
 void Init(){
   set_Output_b5;
 }
 void Do_Process(){
   if(_Blink)
      _Blink = flase;
   else
       _Blink = true;
     
   PORTB_5W(_Blink);
   _delay_ms(100);
 }
 int main(){
   Init();
   while(1){
       Do_Process();
       }
       return 0;
 }
</code></pre>

후에 
CMakeLists.txt 생성 후 작성

<pre><code>

project(${Project_Name} ${Language})
add_executable(${Project_Name} main.c ${Src_Files})

PostBuild_TE()


cmake_minimum_required(VERSION 3.15)

include(TESuit/TEInclude.cmake)

Set_Target(armega328p)
Set_TargetFreq(16000000)
Set_JointInterface(arduino)
Set_JointPort(/dev/ttyACM0)

SET_Project(TESuit_Test)

Init_TE()
Prebuild_TE()

project(${Project_Name} ${Language})
</code></pre>

usb와 아두이노의 연결을 확인 한 후에 실행시키면 LED가 깜빡이는 것을 확인 할 수 있다.
앞으로는 더욱 쉽게 할 수  있도록 시행 중이라고 한다. 오픈소스로 인해 상업화가 힘들기 위해 이런한 생각을 내셨다고 한다.

##### 김두훈 기술이사님에게 짧게 배워 틀린부분과 미흡한 부분이 많습니다. 자세한 내용은 https://github.com/TargetExpert/TargetExpert 확인 하실 수 있습니다. 감사합니다. 
