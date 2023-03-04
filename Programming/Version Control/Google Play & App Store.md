## 重要的操作：Google Play 的 Staged roll-out
![[staged-rollout-example.png]]
這是用來控制該版本會發佈給多少percentage的玩家。
要注意的是，數值雖然不能是0%，但可以是0.0000001%。有這麼多0的話，Installs targeted by rollout 也會是0吧。

___
---
# Review / Upload 伏位
1. **審核需時不可究**
   官方說話 **1 week** 是 **標準**，但通常是3天內便能通過審核，快則是1天之內，也是時有發生的事。
   總結：每當有新的版本，初步檢查過後沒有大問題的話就該把它提交至審核，務求盡快通過審核後，我方能夠重奪主權，隨時能推出該版本。
___
2. **版本不能倒退，而且必須推進**

   Google Play中實際的版本是以Bundle Version Code為準，因此新版本的Bundle Version Code必須比舊版本的大，否則將不能上傳到Google Play Console以發佈新版本。
   Version則是可以一直停留在同一個數值，另外在商店頁面中只有Version會顯示。![[android-identification.png]]
   
   同理，在App Store中，Version可以作停留（但提交審核後則不能，詳細下述），Build則是必須推進，因為Build才是實際的版本。但是該兩個數字均不會在商店頁面中顯示，而是會顯示App Store Connect中的 iOS App 版本。 ![[ios-identification.png]]
   Apple還有一點需要注意，在一個版本通過審核後，該版本的 Version 將不能再重用，而是必須要推進。否則在Xcode上傳新版本時會發生錯誤，要求使用新的 Version 數值。
___
3. **先發佈，後提交審核** (Google Play)

   有時我們同時間有2個版本需要推出（譬如一個是新版本，然後另一個是修復了新版本的一些錯誤和漏洞，但當然要是第2個版本也預留了一週的審核時間則不需要擔心了），假設第1個版本是沒有大問題的，則千萬要先發佈了第1個版本，才把第2個版本提交審核。
   
   Apple 是不用擔心的，因為未發佈第1個版本時是不給提交第2個版本作審核的。
   Google Play 則是很狡猾的：提交第2個版作審核時，會**自動**把未發佈的第1個版本給棄置掉，害我們不能把它發佈，只能夠等第2個版本的審核完成。
___
