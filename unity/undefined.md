# 캐릭터 이동

## 캐릭터의 이동과 애니메이션 처리



### 애니메이터

* 이동에 대한것만 다룰것이다.
* 무료에셋 프리팹의 walk와 Run이 기본으로 등록되어 있어 이걸 트리거로 사용할까 했지만 Speed를 등록해 캐릭터의 이동 속도에 따라 걷거나 뛸수있도록 구현하였다.

<figure><img src="../.gitbook/assets/스크린샷 2023-02-05 오후 11.14.20.png" alt=""><figcaption></figcaption></figure>

* Walk In Place 와 Run In Place 만 확인하면된다.
* Walk In Place
  * Idle -> Walk In Place 방향의 Conditions에 Speed를 등록하고 Greater를 선택, 0.1로 등록
  * 반대로 Idle <- Walk In Place 방향의 Conditions에 Speed를 등록하고 Less를 선택, 0.1로 등록
  * 이렇게하면 스피드가 0.1이상으로 올라갈때 애니메이터 트리거가 작동하여 걷는 모션을 작동하고 반대로 스피드가 0.1아래로 내려가면 Idle상태로 돌아간다
* Run In Place
  * Walk In Place와 연결된 방향의 Conditions도 같은 방식으로 등록해주면 된다. 필자는 Speed가 4이상일때 뛰는 모션으로 변경되도록 하였다
  * 걷다가 뛰거나 뛰다가 걷거나 하는건 되는데 처음부터 뛰는모션으로 가는건 같은 방식으로 하면 잘 안되어 좀더 알아봐야할듯하다. 한마디로 Idle -> Run In Place 를 위와 같은 방식으로 Greater, 4로 작성하면 스피드가 올라갈때 Walk 쪽으로 가버려 적용이 되지 않는다.

<figure><img src="../.gitbook/assets/스크린샷 2023-02-05 오후 11.13.24.png" alt=""><figcaption></figcaption></figure>

### 플레이어 이동 코드 구현

* Rigidbody 등록하고 일단 중력은 체크해제 하였다.
* PlayerMovement.cs 작성하여 프리팹 오브젝트에 연결하였다.
  * 유니티 3D 액션게임\_송호연 님의 책과 인터넷을 참고해 작성함

<figure><img src="../.gitbook/assets/스크린샷 2023-02-05 오후 11.30.54.png" alt=""><figcaption></figcaption></figure>

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(Animator))]
public class PlayerMovement : MonoBehaviour
{
    private Animator avatar;
    private Rigidbody rigidbody;

    public float Speed;
    // Start is called before the first frame update
    void Start()
    {
        avatar = GetComponent<Animator>();
        rigidbody = GetComponent<Rigidbody>();
        Speed = 2f;
    }
    
    //h: Horizontal
    //v: Vertical
    private float h, v;

    public void OnStickChanged(Vector2 stickPos)
    {
        h = stickPos.x;
        v = stickPos.y;
    }
    public void OnKeybordChanged()
    {
        h = Input.GetAxis("Horizontal");
        v = Input.GetAxis("Vertical");
    }
    // Update is called once per frame
    void Update()
    {
        OnKeybordChanged();
        if (avatar)
        {
            avatar.SetFloat("Speed", (Speed*(h*h+v*v)));
            if (rigidbody)
            {
                Vector3 velocity = new Vector3(h, 0, v);
                velocity *= Speed;
                rigidbody.velocity = velocity;
                
                if (h != 0 && v != 0)
                {
                    transform.rotation = Quaternion.LookRotation(velocity);
                }
            }    
        }
        
    }
}

```

* OnStickChanged 는 앞으로 방향컨트롤러를 생성하여 방향을 입력받기 위한 것이고 테스트를 위해 OnKeybordChanged를 생성하여 GetAxis로 방향을 입력 받았다.
* (h\*h+v+v)로 연산된 값은 1이 나온다. 애니메이터의 파라미터인 Speed에 전달하여 애니메이션을 실행한다.
* rigidbody.velocity로 캐릭터의 이동을, transform.rotation으로 방향을 컨트롤한다.





