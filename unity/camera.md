# 플레이어를 따라다니는 카메라

## 캐릭터 움직임에 따라 이동하는 카메라 코드

* 캐릭터로 부터 일정 수치만큼 위에, 일정 수치 만큼 앞에 떨어져 있도록 업데이트한다
* 카메라가 땅을 비추게 하기 위해 rotation 값도 조정함 

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class FollowingCamera : MonoBehaviour
{
    public float distanceAway;

    public float distanceUp;

    public Transform follow;
    
    // Start is called before the first frame update
    void Start()
    {
        distanceAway = 14f;
        distanceUp = 10f;
        transform.rotation = Quaternion.Euler(35, 0, 0);
    }

    // Update is called once per frame
    void LateUpdate()
    {
        transform.position = follow.position + Vector3.up * distanceUp - Vector3.forward * distanceAway;
    }
}
```

## 오브젝트 회전에 관해서

* Rotation은 물체의 회전 각도를 담당한다.
* Quaternion 을 기반으로 한다.
  * x.y,z 축에 가상의 축을 하나 더 추가한 사원수라는 개념
  * 유니티에서 관련 함수를 제공함
  

```csharp
transform.rotation = new Vector3(0,0,0); //error
```
* Quaternion 기반이기 때문에 Position을 초기화하는 것처럼 백터를 넘겨주면 에러가 난다.


```csharp
transform.rotation = Quaternion.Euler(0, 0, 0);
transform.eulerAngles = new Vector3(0,0,0);
```
* 위와 같은 방식으로 물체의 회전을 변경해야한다.