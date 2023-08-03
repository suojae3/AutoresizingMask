
<br/><br/>

## **스토리보드 없이 버튼 객체 생성하기(feat. Autoresizing Mask)**

<br/>

### 00. 스토리보드 없이 객체를 직접 표현할 때 필요한 것은 내용물 + CGRect

- 이때 위치표현에 있어서 디바이스 별로 크기가 다 달라 자동으로 위치를 잡아주는 메서드를 써야합니다
- 이 기능을 도와주는 것이 **`translatesAutoresizingMaskIntoConstraints`** 입니다
- 이 속성은 뷰의 autoresizing mask가 자동레이아웃 제약조건으로 변환되는지 여부를 결정하는 Bool값입니다.

<br/>

#

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


&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://www.thecodedself.com/assets/images/AutoResizing/Autoresizing-Example.gif" width="300" height="100"><br/>

- superview의 bounds가 변경될때 subview의 크기를 어떻게 크기를 재설정 할것인가에 대한 크기조정입니다.
- 여백 간격은 `Top / Left / Bottom / Right Margin` 이라고 부릅니다
- 만약 여백을 눌러 활성화시키면 `Fixed Margin`, 활성화되지 않은 방향을 Flexible Margin 이라고 부릅니다

<br/>

#

### 05. 코드를 통해 구현해주세요

- 아래 첫번째는 autoresizing을 적용하지 않은경우, 두번째는 왼쪽 마진에 autoresizing을 적용한 경우, 세번째는 너비와 높이에 적용한 경우입니다.
- autoresizing을 적용할 때는 `autoresizingMask` 프로퍼티를 사용합니다



&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://blog.kakaocdn.net/dn/bw0uDB/btrcNkzO4lj/kpNokpdaBP2rtIroFrKTp1/img.gif" width="200" height="200"><br/>
autoresize 미적용 <br/><br/>

&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://blog.kakaocdn.net/dn/VLhBV/btrcQfknuSf/78DT0osZKlkVjAlxbG6n20/img.gif" width="200" height="200"><br/>
```swift
//왼쪽 여백 autoresize 적용
sampleSubview.autoresizingMask = [.flexibleLeftMargin]
```

<br/>

&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://blog.kakaocdn.net/dn/dYtZUP/btrcON2JJmT/KnOoYwwQaM2fSM8xnYJTL1/img.gif" width="200" height="200">

<br/>

```swift
// 너비 높이 autoresize 적용
sampleSubview.autoresizingMask = [.flexibleWidth, .flexibleHeight]
```
<br/>

#

### 06. 제약조건을 직접 달아주려면 `AutorizeResizing` 을 false로 두고 시작해야한다!

- 위와 같은 오토리사이징은 서뷰뷰들 위치잡아줄때 쓰면 좋다
- 버튼객체 직접 위치 잡아주고싶을 때는 자동리사이징을 끄고 constraint을 직접 잡아준다
- 가장 먼저 일단 `UIButton()` 을 통해 객체를 생성해준다
    
    ```swift
    var nextButton = UIButton()
    ```
    

- 다음으로 들어가고 싶은 슈퍼뷰에 접근해서 넣어준다.
- 이때는 `**addSubview()**` 메서드를 사용한다
    
    ```swift
    self.view.addSubview(nexButton)
    ```    

- constraint을 잡기 전에 `autoresizing`을 꺼준다
    
    ```swift
    nextButton.translatesAutoresizingMaskIntoConstraints = false
    ```
    
- 스토리보드에서 하던걸 그대로 코드로 구현하면 되는 부분이다
- 버튼을 정가운데 위치시키고 너비300, 높이 30을 설정해보자
- x축(가로), y축(세로) 중앙위치
    
    ```swift
    // Horizontally in Container
    nextButton.centerXAnchor.constraint(equalTo: self.view.centerXAnchor).isActive 
    		= true
    
    // Vertically in Container
    nextButton.centerYAnchor.constraint(equalTo: self.view.centerYAnchor).isActive 
    		= true
    ```
    

- 너비 300, 높이 30 설정
    
    ```swift
    // 버튼 넓이 300
    nextButton.widthAnchor.constraint(equalToConstant: 300).isActive = true
    
    // 버튼 높이 30
    nextButton.heightAnchor.constraint(equalToConstant: 30).isActive = true**
    ```
    

- 위에서 100만큼 왼쪽에서 50만큼 떨어진 제약조건 설정
  ~~~ swift
  //위쪽에서 100
  nextButton.topAnchor.constraint(equalTo: self.view.topAnchor, constant: 100).isActive = true

  //왼쪽에서 50
  nextButton.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 50).isActive = true
  ~~~



