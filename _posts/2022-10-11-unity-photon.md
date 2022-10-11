
## Material 메인 텍스쳐와 노멀맵 offset 방향키로 움직이도록
```C#
public class TrackAnim : MonoBehaviour
{
    private float scrollSpeed = 1.0f;
    private Renderer _renderer;
    // Start is called before the first frame update
    void Start()
    {
        _renderer = GetComponent<Renderer>();
    }

    // Update is called once per frame
    void Update()
    {
        float offset = Time.time * scrollSpeed * Input.GetAxisRaw("Vertical");
        _renderer.material.SetTextureOffset("_MainTex", new Vector2(0, offset));
        _renderer.material.SetTextureOffset("_BumpMap", new Vector2(0, offset));
    }
}
```

## 카메라 움직임에 맞도록 

```C#
public class TurretCtrl : MonoBehaviour
{
    private Transform tr;
    private RaycastHit hit;

    public float rotSpeed = 5.0f;
    // Start is called before the first frame update
    void Start()
    {
        tr = GetComponent<Transform>();
    }

    // Update is called once per frame
    void Update()
    {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        Debug.DrawRay(ray.origin, ray.direction * 100.0f, Color.green);
        if(Physics.Raycast(ray, out hit, Mathf.Infinity, 1 << 8))
        {
            Vector3 relative = tr.InverseTransformPoint(hit.point);
            float angle = Mathf.Atan2(relative.x, relative.y) * Mathf.Rad2Deg;
            tr.Rotate(0, angle * Time.deltaTime * rotSpeed, 0);
        }
    }
}
```

포톤 로그인 후 새 애플리케이션 생성

앱스토어에서 임포트
<br/>

![image](https://user-images.githubusercontent.com/44697751/195003705-ee9a1d5c-0505-4b5b-850e-988b03f311e9.png)

앱 ID 위자드에 프로젝트셋팅에 넣어주고 아래 그림처럼 설정되었는지 확인
<br/>

![image](https://user-images.githubusercontent.com/44697751/195004438-d44a1e4d-873d-4697-a751-1d5c3d1aa217.png)


TCP 소켓 통신하기
```C#
# PhotonInit.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using Photon.Pun;
using Photon.Realtime;

public class PhotonInit : MonoBehaviourPunCallbacks     //포톤에서 제공하는 콜백함수도 쓸 수 있도록
{
    public string version = "1.0v";
    public Text logText;                                //접속 여부 등을 표시할 텍스트

    private void Awake()
    {
        PhotonNetwork.GameVersion = version;            //게임의 버전
        PhotonNetwork.ConnectUsingSettings();           //포톤 클라우드에 접속을 시도
    }
    //포톤 클라우드 접속 성공 시 호출되는 콜백함수
    public override void OnConnectedToMaster()
    {
        Debug.Log("Entered Lobby!");
        PhotonNetwork.JoinRandomRoom();
    }
    //포톤 클라우드 접속 실패 시 호출되는 콜백함수
    public override void OnDisconnected(DisconnectCause cause)
    {
        Debug.Log("Connect Error");
        PhotonNetwork.ConnectUsingSettings();           //포톤 클라우드에 접속을 재시도
    }
    //무작위 룸 접속에 실패한 경우 호출되는 콜백 함수
    public override void OnJoinRandomFailed(short returnCode, string message)
    {
        Debug.Log("No rooms!");
        PhotonNetwork.CreateRoom("My room", new RoomOptions { MaxPlayers = 20 });
    }
    //룸에 입장하면 호출되는 콜백 함수
    public override void OnJoinedRoom()
    {
        Debug.Log("Enter room!");
        CreateTank();                                   //탱크를 네트워크 공간에 생성
    }

    private void Update()
    {
        logText.text = PhotonNetwork.NetworkClientState.ToString();
    }

    void CreateTank()
    {
        float pos = Random.Range(-100f, 100f);
        PhotonNetwork.Instantiate("Tank", new Vector3(pos, 20f, pos), Quaternion.identity, 0);
    }
}

```

탱크에 Photon View 컴포넌트 추가 (로컬인지 리모트인지 검사, 데이터 동기화) 그리고 UDP통신 선택

![image](https://user-images.githubusercontent.com/44697751/195011296-b6861687-a88d-4146-bec4-602ce49e9ef1.png)


탱크에 Photon Transform View 컴포넌트 추가

![image](https://user-images.githubusercontent.com/44697751/195011385-7186f5cb-46c2-4120-a908-8312193fdf53.png)

탱크 움직임 리모트와 원격 으로 나뉘어서 움직이도록 설정!
```C#
#TankMove.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityStandardAssets.Utility;
using Photon.Pun;

public class TankMove : MonoBehaviourPun
{
    public float moveSpeed = 20.0f;                             //이동속도
    public float rotSpeed = 50.0f;                              //회전속도
    //참조 컴포넌트 변수들
    private Rigidbody rbody;  
    private Transform tr;
    //키보드 입력값 변수
    private float h, v;
    private PhotonView pv = null;                                //메인카메라가 추적할 대상(CamPivot 오브젝트)

    public Transform camPivot;

    // Start is called before the first frame update
    void Start()
    {
        rbody = GetComponent<Rigidbody>();
        tr = GetComponent<Transform>();
        pv = GetComponent<PhotonView>();
        if (pv.IsMine)                                          //로컬인지 아닌지 검사
        {
            //로컬인 경우
            Camera.main.GetComponent<SmoothFollow>().target = camPivot;
            rbody.centerOfMass = new Vector3(0.0f, -0.5f, 0.0f);    //탱크의 rigidbody 무게중심 낮게 설정
        }
        else
        {
            rbody.isKinematic = true;
        }
        
    }

    // Update is called once per frame
    void Update()
    {
        if (!pv.IsMine) return;
        h = Input.GetAxis("Horizontal");
        v = Input.GetAxis("Vertical");

        tr.Rotate(Vector3.up * rotSpeed * h * Time.deltaTime);
        tr.Translate(Vector3.forward * v * moveSpeed * Time.deltaTime);
    }
}
```
