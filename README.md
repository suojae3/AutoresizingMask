
<br/><br/>

# **Frame-based Layout and Autoresizing Mask**

<br/>

### 01. 왜 **Frame-based Layout & Autoresizing Mask를 알아야 할까요?**

- 최근에는 Autolayout을 통해 똑같은 기능을 구현할 수 있지만 코드로 UI를 짜거나 프로토타입핑에 적용하기 위해서는  **Frame-based Layout & Autoresizing Mask** 지식이 꼭 필요합니다
- 중간에 UI를 테스트하고 싶은데 각잡고 완성시키기엔 시간이 너무 오래걸리기 때문입니다. **Autoresizing Mask**는 비교적 단시간에 실제 UI를 테스트하는데 요긴하게 쓰입니다.
- 만약 확정된 제약을 추가하면 **Autoresizing Mask**는 자동으로 제거됩니다.
- 중간에 UI를 갈아엎을 것 같거나 끝까지 갈 확률이 희박하다 싶을 때 활용하면 아주 좋습니다

<br/>

#

### 02. Design-time Frame 과 Runtime Frame 이란 무엇인가요?

&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://i.stack.imgur.com/0Hbcr.png" width="400" height="200"><br/>

- Design time은 Xcode에서 편집하는 시점이고 Runtime은 실제로 실행하는 시점입니다
- 뷰의 위치와 크기는 하나로 묶어서 Frame 이라 부릅니다
- Design-time Frame == Runtime Frame 라는 것은 편집할 때의 크기와 실행될 때의 위치와 크기가 같다는 것을 의미합니다.
- 만약 코드로 직접 제약을 추가하면 Size inpector의 프레임 값은 Design-time에서만 사용하고 Runtime frame으로는 사용하지 않습니다

<br/>

#

### 03. **Prototyping Constraints이란 무엇인가요?**

- 만약 IB(인터페이스 빌더)에서 제약을 추가하지 않으면 Xcode는 뷰의 프레임을 기반으로 제약을 자동으로 추가해줍니다. 이러한 제약을 **Prototyping Constraints** 라고 부릅니다
- **Prototyping Constraints**은 스토리보드에 시각적으로 표현되지 않기 때문에 제약을 확인하고 싶다면 코드를 통해 확인해야 합니다
- 만약 코드로 직접 제약을 추가하면 **Prototyping Constraints**이 사라집니다.

<br/>

#

### 04. Autoresizing Mask란 무엇인가요?


&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://www.thecodedself.com/assets/images/AutoResizing/Autoresizing-Example.gif" width="400" height="200"><br/>

- superview의 bounds가 변경될때 subview의 크기를 어떻게 크기를 재설정 할것인가에 대한 크기조정입니다.
- 여백 간격은 `Top / Left / Bottom / Right Margin` 이라고 부릅니다
- 만약 여백을 눌러 활성화시키면 `Fixed Margin`, 활성화되지 않은 방향을 Flexible Margin 이라고 부릅니다
