
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


