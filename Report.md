# รายงานผลการทดลอง

###### <นายรุ่งโรจน์  พลเยี่ยม> <603410060-4>

## คำสั่งการแสดงผลผ่าน Logcat
```kotlin
import android.util.Log
```
Debug log

```kotlin
Log.d("tag:", "msg:"); 
```

Error log 

```kotlin
Log.e("tag:", "msg:");  
```

Info log 

```kotlin
Log.i("tag:", "msg:");  
```

Verbose log 

```kotlin
Log.v("tag:", "msg:");  
```

Warning log 

```kotlin
Log.w("tag:", "msg:"); 
```

## SNACKBAR และ TOST

คำสั่งแสดง Snackbar

```kotlin
Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
```

คำสั่งแสดง Tost

```kotlin
Toast.makeText(this, "Replace with your own action", Toast.LENGTH_SHORT).show();
```

## Android LiveCycle Activity

จงอธิบาการทำงานของเมธอทต่อไปนี้ ว่าเกิดขึ้นเมื่อใดของโปรแกรม พร้อมแสดงตัวอย่างโค้ดการทำงานของเมธอท (กำหนดให้แสดง log message เมื่อมีการทำงานของเมธอท)

1. onCreate() -> Android จะเรียก onCreate() เมื่อ Activity Start ในหนึ่งช่วงเวลาของ Application นั้น อาจมีการ Create และ Destroy Activity อยู่เรื่อยๆ ยกตัวอย่างเช่น เมื่อ User ทำการ Rotate Screen จะส่งผลให้ Activity ถูก Destroy และ Instance ใหม่ของ Activity เดิมก็จะถูก Create อีกครั้ง

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        setSupportActionBar(toolbar)
        Log.i("onCreate","Activity created")
```

2. onStart() ->  หลัง จาก onCreate() ถูกเรียก onStart() ก็จะถูกเรียกตามมา ถ้าหาก Application ของเราถูกสั่งให้ไปอยู่ใน Background (อาจจะโดยการสั่ง Launch Application อื่น เหนือ Application ของเรา) onStart() จะถูกเรียกเมื่อเรากลับมาที่ Application อีกครั้ง

```kotlin
override fun onStart() {
        super.onStart()
        Log.i("lifecycle Start", "onStart")
        }
```

3. onResume() ->เมื่อ Application ของเราถูก Start ขึ้นมา onResume() จะถูกเรียกหลังจาก onCreate() และ onStart() หรือเมื่อ Application เปลี่ยนสถานะจาก Background กลับมาอยู่บน Foreground อีกครั้ง onResume() ก็จะถูกเรียกเช่นเดียวกัน onResume() จะถูกเรียก ก่อนที่ Activity จะสามารถมองเห็นได้บน Screen

```kotlin
override fun onResume() {
        super.onResume()
        Log.i("onResume", "lifecycle resume")
        }
```

4. onPause() ->onPause() จะถูกเรียก เมื่อ Application กำลังเปลี่ยนจากสถานะ Foreground ไปยัง Background ถ้าหากเรามีการประมวลผลอะไรก็ตามที่ควรจะ Run เฉพาะตอนที่ Activity อยู่บน Screen (ยกตัวอย่างเช่น การแสดง Animation) เราควรจะสั่งหยุดการประมวลผลดังกล่าวใน onPause()

```kotlin
override fun onPause() {
        super.onPause()
        Log.i("lifecycle Pause", "onPause")
        }
```

5. onStop() ->onStop() จะถูกเรียกเมื่อ Activity ออกจาก Screen เรียบร้อยแล้ว หรือเมื่อเราเปลี่ยนไป Interact กับอีก Activity หนึ่งแทน (เปลี่ยนจากสถานะ Active ไปเป็น Inactive) แต่นี่ไม่ได้หมายความ Activity ถูกปิดตัวลง เพียงแค่เปลี่ยนมาอยู่ในสถานะ Inactive ถ้าหากเรามีการประมวลผลใดที่ต้องการ Run เฉพาะตอนที่ Active นี่คือ Method ที่เหมาะสมที่เราจะสั่งหยุดการประมวลผลดังกล่าว

```kotlin
override fun onStop() {
        super.onStop()
        Log.i("lifecycle Stop", "onStop")
        }
```

6. onDestroy() ->onDestroy() เป็น method สุดท้ายที่จะถูกเรียก ก่อนที่ Activity จะปิดตัวลงอย่างถาวร เป็น Method ที่เราใช้ในการคืนค่า resources หรือ clean up ใดๆ ก่อนที่ Activity จะถูกเก็บกวาดด้วย Garbage Collector ภายใน Method นี้ เราควรจะสั่งปิด Process ใดๆที่เราสั่ง Run ไว้ใน Background

```kotlin
override fun onDestroy() {
        super.onDestroy()
        Log.i("lifecycle Destroy", "onDestroy")
        }
```

7. onRestart() -> เมธอท ตัวนี้จะถูกเรียกใช้งานเมื่อ Activity ถูก restart 

```kotlin
override fun onRestart() {
        super.onRestart()
        Log.i("lifecycle Restart", "onRestart")
        }
```

## Start new Activity

คำสั่งสำหรับเปิด activity ใหม่

```kotlin
var i = Intent(this,Main2Activity::class.java)
            startActivity(i)
```

คำสั่งสำหรับเปิด activity ใหม่ ผ่านเมนู setting

```kotlin
   override fun onOptionsItemSelected(item: MenuItem): Boolean {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        return when (item.itemId) {
            R.id.action_settings -> {
                var i = Intent(this,Main2Activity::class.java)
                startActivity(i)
                return true
            }
            R.id.action_settings -> true
            else -> super.onOptionsItemSelected(item)
        }
    }
```

