---
layout: single
title:  "3D Game Kit을 사용하여 커스텀 개임을 개발하기 2탄!"
---


# 3D Game Kit을 사용하여 커스텀 개임을 개발하기 - 2탄 개발하기
<br>

<details>
<summary>접기/펼치기</summary>

#### 유니티 버전 : 2021.3.12f1 : 1년 전 중 가장 최신
<br>

##### 1) Package 설치:
- X https://github.com/febucci/unitypackage-custom-hierarchy : Custom Hierarchy for Unity(설치불가)
- O 3D Game Kit 
<br>

##### 2) 프로젝트 파일 설정
- O 안드로이드 변경 & 패키지 설치(Post Processing, Joystick Pack, Standard Assets)
<br>

##### 3) Layout 설정: 
- PJH.wlt 파일 사용
<br>
  
</details>
<br>
  
           
# O 캐릭터이동 및 카메라 앵글이동(조이스틱 및 터치 로테이션)
<details>
<summary>접기/펼치기</summary>

#### 1. 카메라 앵글이동 : https://www.youtube.com/watch?v=bk19NYT_ZIY&ab_channel=SahaniStudio 참조
##### 1) 기존 카메라 앵글이동 끄기
<details>
<summary>StartUI.cs</summary>

```cs
# Assets/3DGamekit/Scripts/Game/UI/StartUI.cs

        void Start()
        {
            if (!alwaysDisplayMouse)
            {
                // 원래
                //Cursor.lockState = CursorLockMode.Locked;
                //Cursor.visible = false;
                Cursor.lockState = CursorLockMode.None;
                Cursor.visible = true;
            }
            ...
```
</details>
<br>
     
##### 2-1) 터치할 영역 설정 : 패널에서
- Hierachy > Canvas > Panel > RectTransform 화면 반으로 설정
- Panel > TouchField.cs 컴포넌트 추가
<details>
<summary>TouchField.cs</summary>

```cs
# TouchField.cs

using UnityEngine.EventSystems;
using UnityEngine;

public class TouchField : MonoBehaviour, IPointerDownHandler, IPointerUpHandler
{
    [HideInInspector]
    public Vector2 TouchDist;
    [HideInInspector]
    public Vector2 PointerOld;
    [HideInInspector]
    protected int PointerId;
    [HideInInspector]
    public bool Pressed;

    // Use this for initialization
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        if (Pressed)
        {
            if (PointerId >= 0 && PointerId < Input.touches.Length)
            {
                TouchDist = Input.touches[PointerId].position - PointerOld;
                PointerOld = Input.touches[PointerId].position;
            }
            else
            {
                TouchDist = new Vector2(Input.mousePosition.x, Input.mousePosition.y) - PointerOld;
                PointerOld = Input.mousePosition;
            }
        }
        else
        {
            TouchDist = new Vector2();
        }
    }

    public void OnPointerDown(PointerEventData eventData)
    {
        Pressed = true;
        PointerId = eventData.pointerId;
        PointerOld = eventData.position;
    }


    public void OnPointerUp(PointerEventData eventData)
    {
        Pressed = false;
    }

}
``` 
</details>
<br>
    
##### 2-2) 터치할 영역 설정 : 씨네머신에서
- Hierachy > CameraRig > KeyboardAndMouseFreeLookRig > CineTouch.cs 추가
- Hierachy > CameraRig > KeyboardAndMouseFreeLookRig > CinemachineFreeLook > Y Axis & X Axis > Input Axis Name > 지워 비우기
<details>
<summary>CineTouch.cs</summary>

```cs
# CineTouch.cs

using UnityEngine;
using Cinemachine;

public class CineTouch : MonoBehaviour
{
    [SerializeField] CinemachineFreeLook cineCam;
    [SerializeField] TouchField touchField;
    [SerializeField] float SenstivityX = 2f;
    [SerializeField] float SenstivityY = 2f;

    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        cineCam.m_XAxis.Value += touchField.TouchDist.x * 200 * SenstivityX * Time.deltaTime;
        cineCam.m_YAxis.Value += touchField.TouchDist.y * SenstivityY * Time.deltaTime;
    }
}
``` 
</details>
<br>

#### 2. 캐릭터 이동
##### 1) 이동 조이스틱 : https://www.youtube.com/watch?v=GGqwMGZiwCg&ab_channel=%EA%B3%A8%EB%93%9C%EB%A9%94%ED%83%88 참조
<details>
<summary>PlayerInput.cs</summary>

```cs
# Assets/3DGamekit/Scripts/Game/Player/PlayerInput.cs
  
using UnityStandardAssets.CrossPlatformInput;

    public VariableJoystick vJoyMovement;

    void Update()
    {
      m_Movement.Set(vJoyMovement.Horizontal, vJoyMovement.Vertical);
      ...
      m_Jump = CrossPlatformInputManager.GetButton("Jump");
      ...
      if (CrossPlatformInputManager.GetButtonDown("Fire1"))
      ...
``` 
</details>
<br>
  
##### 2) 점프 및 공격 버튼 : https://www.youtube.com/watch?v=SOL0ABaLwAE&t=266s&ab_channel=SahaniStudio 참조
<br>

</details>
<br>
  

# O 로그인 구현
<br>

# O (대기) 홈화면 구현
  -db에 들어갈 단어 10개
<br>
  
# O 게임1 구현(기본 게임플레이)
<br>

## O 게임1 : TCP Chat 구현
<br>

## @ 게임1 : Speaking 구현
<br>
  
  ```cs
  # Speech to Text
  # SpeechToText.cs
  
  # 첫번째로 녹음 파일 만들기
  # Microphone을 활용
  
  ```

## 게임1 : Writing 구현
<br>

## 게임1 : 단어 추천 및 학습 단어 현황 구현
<br>
  
## 게임1 : 멀티플레이 구현 : 위와 동일하게 구현
<br>

# 게임2 구현(기본 게임플레이)
<br>
  
## 게임2 : TCP Chat 구현
<br>
