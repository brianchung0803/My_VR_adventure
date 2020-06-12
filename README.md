# VR Tutorial :eyeglasses:
* Class
  * first class: Roll a Ball

# first class: Roll a Ball
* 介紹:
> 建立一個簡易的pick up遊戲，玩家為一個可以控制的球並要控制此球撿取散落的物體

* step 1 
> 建立新的project，選取3D並命名為roll a ball tutorial
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/create%20project.png?raw=true" alt="Sample"  width="550" height="350">
</p>

* step 2 
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
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/create%20material.png?raw=true" alt="Sample"  width="300" height="350">
</p>

> 調整ground的material的顏色並拖曳至Scene中的ground
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20ground%20material.png?raw=true" alt="Sample"  width="300" height="350">
</p>
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20ground%20materals.png?raw=true" alt="Sample"  width="550" height="350">
</p>

> 調整光源
<p align="left">
    <img src="https://github.com/brianchung0803/My_VR_adventure/blob/master/images/set%20light.png?raw=true" alt="Sample"  width="550" height="350">
</p>

* step 3
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
