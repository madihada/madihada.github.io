---
layout: single
title:  "3D Game Kit을 사용하여 커스텀 개임을 개발하기 2탄!"
categories: unity
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
  
  스피킹 구현(ETRI API)
  ETRI API를 사용하기 위해서는  
  첫번째, 음성 녹음 & 파일 저장  
    1. 음성 녹음  
    2. 파일 저장  
  먼저 파일은 저장하기 위해서는 아래 SavWav.cs를 오브젝트에 붙여준다
<details>
<summary>접기/펼치기</summary>
  
```cs
# SavWav.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
using System.IO;
using System.Collections.Generic;

public static class SavWav
{

	const int HEADER_SIZE = 44;

    public static bool Save(string filename, AudioClip clip, bool makeClipShort = true)
    {
        if (!filename.ToLower().EndsWith(".wav"))
        {
            filename += ".wav";
        }

        var filepath = Path.Combine(Application.dataPath, filename);


        Debug.Log(filepath);

        // Make sure directory exists if user is saving to sub dir.
        Directory.CreateDirectory(Path.GetDirectoryName(filepath));

        if (makeClipShort)
        {
            clip = TrimSilence(clip, 0);
        }

        using (var fileStream = CreateEmpty(filepath))
        {
            ConvertAndWrite(fileStream, clip);

            WriteHeader(fileStream, clip);
        }

        return true; // TODO: return false if there's a failure saving the file
    }

	public static AudioClip TrimSilence(AudioClip clip, float min)
	{
		var samples = new float[clip.samples];

		clip.GetData(samples, 0);

		return TrimSilence(new List<float>(samples), min, clip.channels, clip.frequency);
	}

	public static AudioClip TrimSilence(List<float> samples, float min, int channels, int hz)
	{
		return TrimSilence(samples, min, channels, hz, false, false);
	}

	public static AudioClip TrimSilence(List<float> samples, float min, int channels, int hz, bool _3D, bool stream)
	{
		int i;

		for (i = 0; i < samples.Count; i++)
		{
			if (Mathf.Abs(samples[i]) > min)
			{
				break;
			}
		}

		samples.RemoveRange(0, i);

		for (i = samples.Count - 1; i > 0; i--)
		{
			if (Mathf.Abs(samples[i]) > min)
			{
				break;
			}
		}

		samples.RemoveRange(i, samples.Count - i);

		var clip = AudioClip.Create("TempClip", samples.Count, channels, hz, _3D, stream);

		clip.SetData(samples.ToArray(), 0);

		return clip;
	}

	static FileStream CreateEmpty(string filepath)
	{
		var fileStream = new FileStream(filepath, FileMode.Create);
		byte emptyByte = new byte();

		for (int i = 0; i < HEADER_SIZE; i++) //preparing the header
		{
			fileStream.WriteByte(emptyByte);
		}

		return fileStream;
	}

	static void ConvertAndWrite(FileStream fileStream, AudioClip clip)
	{

		var samples = new float[clip.samples];

		clip.GetData(samples, 0);

		Int16[] intData = new Int16[samples.Length];
		//converting in 2 float[] steps to Int16[], //then Int16[] to Byte[]

		Byte[] bytesData = new Byte[samples.Length * 2];
		//bytesData array is twice the size of
		//dataSource array because a float converted in Int16 is 2 bytes.

		int rescaleFactor = 32767; //to convert float to Int16

		for (int i = 0; i < samples.Length; i++)
		{
			intData[i] = (short)(samples[i] * rescaleFactor);
			Byte[] byteArr = new Byte[2];
			byteArr = BitConverter.GetBytes(intData[i]);
			byteArr.CopyTo(bytesData, i * 2);
		}

		fileStream.Write(bytesData, 0, bytesData.Length);
	}

	static void WriteHeader(FileStream fileStream, AudioClip clip)
	{

		var hz = clip.frequency;
		var channels = clip.channels;
		var samples = clip.samples;

		fileStream.Seek(0, SeekOrigin.Begin);

		Byte[] riff = System.Text.Encoding.UTF8.GetBytes("RIFF");
		fileStream.Write(riff, 0, 4);

		Byte[] chunkSize = BitConverter.GetBytes(fileStream.Length - 8);
		fileStream.Write(chunkSize, 0, 4);

		Byte[] wave = System.Text.Encoding.UTF8.GetBytes("WAVE");
		fileStream.Write(wave, 0, 4);

		Byte[] fmt = System.Text.Encoding.UTF8.GetBytes("fmt ");
		fileStream.Write(fmt, 0, 4);

		Byte[] subChunk1 = BitConverter.GetBytes(16);
		fileStream.Write(subChunk1, 0, 4);

		UInt16 two = 2;
		UInt16 one = 1;

		Byte[] audioFormat = BitConverter.GetBytes(one);
		fileStream.Write(audioFormat, 0, 2);

		Byte[] numChannels = BitConverter.GetBytes(channels);
		fileStream.Write(numChannels, 0, 2);

		Byte[] sampleRate = BitConverter.GetBytes(hz);
		fileStream.Write(sampleRate, 0, 4);

		Byte[] byteRate = BitConverter.GetBytes(hz * channels * 2); // sampleRate * bytesPerSample*number of channels, here 44100*2*2
		fileStream.Write(byteRate, 0, 4);

		UInt16 blockAlign = (ushort)(channels * 2);
		fileStream.Write(BitConverter.GetBytes(blockAlign), 0, 2);

		UInt16 bps = 16;
		Byte[] bitsPerSample = BitConverter.GetBytes(bps);
		fileStream.Write(bitsPerSample, 0, 2);

		Byte[] datastring = System.Text.Encoding.UTF8.GetBytes("data");
		fileStream.Write(datastring, 0, 4);

		Byte[] subChunk2 = BitConverter.GetBytes(samples * channels * 2);
		fileStream.Write(subChunk2, 0, 4);
	}
}
```
</details>
<br>
  
  두번째, 녹음 파일을 byte로 변환 & base64 포멧
  세번째, header & body 파일 보내기
  
<details>
<summary>접기/펼치기</summary>
  
  ```cs
using Newtonsoft.Json;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;


public class BodyData
{
    public Dictionary<string, string> argument;

    public BodyData(Dictionary<string, string> argument)
    {
        this.argument = argument;
    }
}
public class PostJsonToServer : MonoBehaviour
{
    string url = "http://aiopen.etri.re.kr:8000/WiseASR/Recognition";

    void Start()
    {
        Dictionary<string, string> bodyDick = new Dictionary<string, string>
        {
            { "language_code", "english" },
            { "audio", "" }
        };

        BodyData dataObject = new BodyData(bodyDick);

        //string json = JsonUtility.ToJson(bodyDick);
        string json = JsonConvert.SerializeObject(dataObject);

        StartCoroutine(Upload(url, json));
    }
    

    IEnumerator Upload(string URL, string json)
    {
        // 1.POST할 파일 form만들기


        // 2.POST하기 (Server에서 result 생성하고 보내줌)
        using (UnityWebRequest www = UnityWebRequest.Post(URL, json))
        {
            byte[] jsonToSend = new System.Text.UTF8Encoding().GetBytes(json);
            www.uploadHandler = new UploadHandlerRaw(jsonToSend);
            www.downloadHandler = (DownloadHandler)new DownloadHandlerBuffer();
            www.SetRequestHeader("Content-Type", "application/json");
            www.SetRequestHeader("Authorization", "e0caa335-d7cb-4099-b054-9125c197911c");

            yield return www.SendWebRequest();
            // 3.request 후 response 받기
            var jsonResponse = www.downloadHandler.text;  //request 후 response 받기
            Debug.Log("response is ... " + jsonResponse);
        }
    }
}

  ```
</details>
<br>
  
  
  ```cs
  # Speech to Text
  # SpeechToText.cs
  
  # 첫번째로 녹음 파일 만들기
  # Microphone을 활용
  
  ```
  
  
  ### 두번째 몬스터에 접근하면 몬스터가 가진 퀴즈 팝업으로 질문 & 대답
  
  원리,,
  ```cs
  # MonsterController.cs
  1. OnCollisionEnter()                     // 충돌체크
  1-1. canvas.SetActive(true);              // InputController로 이동
  
  # InputController.cs
  2. OnEndEditEvent()                                     // Input Field에 입력값 받아와서 정답 체크
  2-2. GameManager.instance.IsMonsterOver = true;         // GameManager을 통해서 MonsterController 이동 하여 파괴 준비
  
  # GameManager.cs
  3. public bool IsMonsterOver Invoke("DeleteMonster", 0.5f);       // 프로퍼티로 델리게이트 소환
  3-1. public void DeleteMonster() OnMonsterDie();                  // 델리게이트로 다른 스크립트 함수 실행
  
  # MonsterController.cs
  4. public void DeleteMonster() Destroy(willDestroyObject);        // 해당 몬스터 파괴

  ```
  ###########################################################################################################################
  ### 세번째 팝업퀴즈 정답 맞추기!
  1. 퀴즈에 나타날 내용들
    a. 한글문장 (db에 값을 넣을 때 컬러태그를<color> 포함하여 넣어야할까?)             //yes
    b. 영어문장 (정답 단어에 길이의 맞도록 input field 넓이를 조절할 수 있을까?        //yes
    c. 힌트1단계 2단계 3단계 (변수 내용 바뀌면 실시간으로 적용 어떻게)                 //q3 
    d. 정답이 아닌 유의어이면 틀렸다는 표시와 위에 말풍선 넣기 어떻게 에니메이션?
    e. 유의어 넣게 유의어 입력시 나타날 내용들
    f. 
  
  2. 퀴즈 정답 입력
    a. 정답이 맞으면 특정 함수 실행                                       
  
  
  
  ###########################################################################################################################
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
