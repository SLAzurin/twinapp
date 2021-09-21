Source: https://zentalk.asus.com/zh/discussion/12875/%E8%87%AA%E8%A1%8C%E4%BF%AE%E6%94%B9%E6%87%89%E7%94%A8%E5%88%86%E8%BA%AB%E6%94%AF%E6%8F%B4-app-%E5%88%97%E8%A1%A8#latest

原理在兩年前的文章 [ZenFone] Add new apps to Twin Apps 有說明了，這邊直接給檔案跟步驟：

需要檔案：twinapps_required_apps.xml，網頁點選『raw』後把內容存成 twinapps.xml


如何加入新的應用程式？

1. 先找出應用程式的 package name，最簡單的方法就是去網頁版 play store 查詢，好比說我想加入 GoShare 這個 app，他的 play store 網址是： 
https://play.google.com/store/apps/details?id=com.gogoro.goshare 裡面 id= 接的就是他的 package name，以 GoShare 來說就是 com.gogoro.goshare

2. 編輯 twinapps.xml，找到 `<string-array name="twinapps_required_apps">` 這個區塊，或者是 `<string-array name="twinapps_required_apps_games">` 也行 (遊戲)，把前面查詢到的 package name 用 `<item></item>` 包起來放在該區塊裡面。以 GoShare 來說，我可以在 twinapps_required_apps 裡面最後一個項目 `<item>com.imo.android.imoim|imo free video calls and chat</item>` 後面插入 `<item>com.gogoro.goshare</item>`，然後存檔。

3. 手機接上電腦利用 adb 下指令把 twinapps.xml 放在內部儲存空間： 
```
adb push twinapps.xml /sdcard/
```

4. 再透過 adb 指令更新應用分身的支援列表： 
```
adb shell am startservice -a "asus.intent.action.TWINAPPS_CDN_FILE_UPDATE" -d "file:///sdcard/twinapps.xml" --ei "ACTION" 1 com.asus.twinapps/.TwinAppsService
```


如果沒出現新增的項目，或者一片空白該如何處理？

 - 有可能是因為輸入的內容有錯，比如 " 變成 ”，或者多了空白等等。 此時可以先重新下載最原始的 twinapps.xml 然後更新一次即可恢復。
 - 也有可能是因為檔案編碼格式有問題，建議不要用 copy paste 的方式建立 twinapps.xml，而是直接儲存 raw 頁面。


我可以刪除不要的應用程式項目嗎？
 - 你可以直接把看不順眼的 <item>應用程式項目</item> 整段刪除，然後再透過前面的更新指令更新即可。

如果加入之後程式跑起來怪怪的 (閃退) 該怎麼辦？

 - 因為是 hack 所以無法完全支援.. 很正常，只能碰碰運氣了。


Happy Hacking !