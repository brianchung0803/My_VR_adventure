# VR Tutorial :eyeglasses:
* Unity
  * first lesson: Roll a Ball

## :pencil2:first lesson: Roll a Ball
* 介紹:
> 建立一個簡易的pick up遊戲，玩家為一個可以控制的球並要控制此球撿取散落的物體

* step 1 建立新project
> 建立新的project，選取3D並命名為roll a ball tutorial
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/create%20project.png?raw=true" alt="Sample"  width="550" height="350">
</p>

* step 2 基本物件
> 建立play field，點選GameObject-> 3D Object ->Plane，並命名為ground
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/create%20ground.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> 每一次在建立物件時都記得要點reset，使物體矯正至原點
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/reset%20ground.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> 更改ground的size，大小調整成x=2,y=1,z=2(因為是plane，所以y的大小沒有影響)
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20ground%20size.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> 建立一個sphere，並命名為player
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/create%20player.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> reset後將position改成y=0.5，使球放置在ground上
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20player%20position.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> 建立一個新的檔案夾，命名為Materials，在裡面放置要新增的material，第一個建立ground的material
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/create%20material.png?raw=true" alt="Sample"  width="300" height="450">
</p>

> 調整ground的material的顏色並拖曳至Scene中的ground
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20ground%20material.png?raw=true" alt="Sample"  width="300" height="450">
</p>
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20ground%20materals.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> 調整光源
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20light.png?raw=true" alt="Sample"  width="550" height="350">
</p>

* step 3 寫script使player移動
> 要使player移動，需要將player加上物理運算。選取物體並在Inspector的Add Component，點選Physics->Rigidbody

> 建立一個Scripts檔案並將要寫的script存在裡面，並在player的Inspector的Add Component->New Script並命名為PlayerController來使用鍵盤控制移動
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayController : MonoBehaviour
{
    public float speed; //設定一個可以更改的速度參數
    private Rigidbody rb;
    
    void Start()
    {
        rb=GetComponent<Rigidbody>();
    }
    void FixedUpdate()
    {
        float moveHorizontal =Input.GetAxis("Horizontal");
        float moveVertical =Input.GetAxis("Vertical"); //上面兩行可以得知鍵盤輸入
        Vector3 movement = new Vector3(moveHorizontal,0.0f,moveVertical); //設定(x,y,z)的力
        rb.AddForce(movement*speed);
    }

}
```
補充:[FixedUpdate與Update的差異](http://www.victsao.com/blog/97-unity/420-unity-update-fixupdate)

> 回到unity interface設定speed為10，並進行測試
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20speed.png?raw=true" alt="Sample"  width="550" height="350">
</p>

* step 4 設定視角
> 建立第三人稱視角，要小心不要直接將Main Camera直接加入player的child，因為player滾動則camera也回跟著滾動。正確的方式在Main Camera新增一個CameraController的script並設定和player之間有x,y,z距離的offset
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{
    public GameObject player;
    private Vector3 offset;

    // Start is called before the first frame update
    void Start()
    {
        offset = transform.position - player.transform.position;
    }

    void LateUpdate() //update after all items are updated
    {
        transform.position = player.transform.position + offset;
    }
}
```
補充: 使用LateUpdate是希望能在player移動後(frame與frame之間)，camera再移動

> 回到Unity interface，將Hierarchy中的player拖曳至Main Camera的Player欄位(上方程式新增的)

* step 5 建立遊戲空間
> 建立四周的Wall。點選GameObject內的Create Empty，命名為Walls並reset

> 創一個新的cube，命名為westwall，reset後將他伸縮(0.5,2,20.5)並移動至邊緣(-10,0,0)

> 將westwall複製後建立其他三個邊界
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/add%20walls.png?raw=true" alt="Sample"  width="550" height="350">
</p>

* step 6 建立收集物品
> 創建要收集的東西。

> 建立一個cude取名為pickup，reset後將position改為(0,0.5,0)，rotation改為(45,45,45)，scale改為(0.5,0.5,0.5)

> 接下來增加一個script來使他在原地旋轉

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class rotator : MonoBehaviour
{
    void Update()
    {
        transform.Rotate(new Vector3(15,30,45)*Time.deltaTime);
    }
}
```
> 創建一個檔案夾Prefabs，並將pickup拖曳至檔案夾儲存，如此一來便能在其他scene使用這個物件

> 點選GameObject內的Create Empty，命名為pickups並reset，將剛剛創建的pickup設成child後，複製pickup並隨意擺放。最後再新建一個material，拖曳至任一一個pickup，並選取apply all以改變所有的pickups。

<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/prefab%20apply.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> 接下來設定player碰到pickup會使pickup消失，在player的PlayerController script內新增:
```cs
void OnTriggerEnter(Collider other) // OnTriggerEnter會偵測物體碰觸到其他Collider
    {
        if(other.gameObject.CompareTag("Pick UP"))
        {
            other.gameObject.SetActive(false);
        }
    }
```

> 點選Prefabs檔案夾內的pickup，並在Tag欄位點選Add Tags，新增一個Pick UP(要和PlayerController.cs內寫的一樣)，最後將Box Collider的Is Trigger點選，進入遊玩時，player碰到pickup便會使pickup消失
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/game%20play.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> 因為pickup在Unity內是存成static collider，而這些static collider在scene中會存成一個cashe，因為static collider理論上不會動，所以cashe不會更新，但是因為我們讓pickup每一偵都會旋轉，故cashe會不斷更新，造成運算浪費。要解決此問題只要將pickup增加Rigidbody，pickup便會被視為dynamic collider，增加Rigidbody後，將Use Gravity取消，並勾選Is Kinematic。

<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/pickup%20rigidbody.png?raw=true" alt="Sample"  width="300" height="450">
</p>

* step 7 建立UI
> 打開player的PlayerController.cs，紀錄撿了幾個物品
```cs
//新增一個count並在start()設為0
private int count;
    void Start()
    {
        rb=GetComponent<Rigidbody>();
        count=0;
    }
//在OnTrigger內計算
 void OnTriggerEnter(Collider other)
    {
        if(other.gameObject.CompareTag("Pick UP"))
        {
            other.gameObject.SetActive(false);
            count+=1;
        }
    }
```
> 創建一個UI的Text命名為Count Text並reset，更改成白色以方便閱讀。

> 調整Anchor Presets，同時按Shift及Alt，並選取左上角，使計分固定在畫面左上方，再微調position

> 更改player的PlayerController.cs
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI; //新增UnityEngine.UI

public class PlayController : MonoBehaviour
{
    public float speed; 
    private Rigidbody rb;
    private int count;
    public Text CountText; //新增一個variable

    void Start()
    {
        rb=GetComponent<Rigidbody>();
        count=0;
        setcount(); //初始顯示
    }
    void FixedUpdate()
    {
        float moveHorizontal =Input.GetAxis("Horizontal");
        float moveVertical =Input.GetAxis("Vertical"); //上面兩行可以得知鍵盤輸入
        Vector3 movement = new Vector3(moveHorizontal,0.0f,moveVertical); //設定(x,y,z)的力
        rb.AddForce(movement*speed);
    }

    void OnTriggerEnter(Collider other)
    {
        if(other.gameObject.CompareTag("Pick UP"))
        {
            other.gameObject.SetActive(false);
            count+=1;
            setcount(); //顯示改變
        }
    }

    void setcount() //用來顯示的function
    {
        CountText.text="Count:"+count.ToString();
    }
}
```
> 回到unity的interface將Count Text拖曳至player新增的variable

> 再新建一個UI的Text命名為Win Text，reset後更改成白色後放大字體以方便閱讀並將位置設置在中間偏上

> 更改player的PlayerController.cs

```cs
public Text WinText; //新增一個variable

void Start()
{
   rb=GetComponent<Rigidbody>();
   count=0;
   WinText.text=""; //在start()新增初始顯示
   setcount();
}
   
void setcount()
{
    CountText.text="Count:"+count.ToString();
    if(count>=8) //新增勝利條件(我放8個pickups)
    {
        WinText.text="You Win!!"; //顯示勝利
    }
}

```
> 回到unity的interface將Win Text拖曳至player新增的variable

* step 8 build game
> 儲存後選取file的build settings。選取Platform後將做好的Scene放至上方的Scene In Build，設置好後按build
