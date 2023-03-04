# Interview questions

> [!question]- Why quit your previous job?
> 我會歸因於就係，我知道自己唔係好為份工做好準備；
> 
> 成段故就是咁的：我喺讀書嗰陣就掂過下Unity，整過幾款小遊戲，然後就得前老闆賞識同請咗我做 Programmer，但係呢度有個伏筆就係當其時我雖然識得用Unity，但係對 Programming 嘅認識就唔算好深。
> 
> 然之後我就懷住一份積極又有點不安嘅心情，我就開始返工喇。然後就開始見識到啲production level 嘅 code，而呢種 project scale 係我未面對過嘅嘢嚟。不過我同自己講其實唔洗怯嘅，反正做落咗熟悉咗份 project 啲嘢咪會去得順返囉。
> 
> 結果都係真嘅，份工係會愈做愈順嘅。所以整體嚟講其實份工都幾乎合我期望，都係有時準時收工有時OT，有時係fix bug，有時係搞下新意思，而我一直都學下新嘢咁樣。不過間唔時都係會有啲關於 project management 上嘅 struggles 出現，所以呢度一直都留咗一條刺，令到我會覺得自己仍然唔係嗰個能夠處理呢種 scale 嘅 programmer。
> 
> 到後來我覺得，好喇咁我正正經經地去學 computer science。然之後過咗陣我先至有 data-driven design，automated testing，CI/CD，gaming service 呢啲概念。或者就算個挑戰唔係喺 project management 呢方面嘅，我都會識得搵啲適合嘅工具去幫我手去解決問題。
> 
> 所以依家我可以話，okay i'm ready。

> [!question]- Why making games?
> 首先係我對創作類型嘅工作係有一份憧憬。雖然好長一段時間我冇付出行動去創作，不過就一直都俾好多動漫、漫畫、電影、遊戲薰陶緊啦。
> 
> 然後某日喺我覺得好迷失好懷疑人生嘅時候，唔知點解我仲有心機打開 dark souls 嚟玩，我就話：okay 既然我喜歡呢樣嘢，咁 why not 整 game 呢？完全係一個唔係好知自己想做乜嘅人會諗嘅嘢。
> 
> 到依家我接觸過整遊戲喇，我知道咗好多 why not 整 game 嘅理由喇。我依然會係俾遊戲呢樣嘢吸引住，依家係因為我知道咗一啲遊戲背後嘅故事，我有份見證一個 idea 點樣變成一個遊戲再變成受人熱讚嘅故事。之後就返唔到轉頭喇，我都想成為其中一個。

> [!question]- Talk through your experience in the industry so far.
> 我由 IVE - THEi 讀 Multimedia 拎到個學士學位畢業。過程中因為我識得用 Unity 而有幸返到一份遊戲開發嘅 part-time 工作，喺呢度就拎到大概一年嘅工作經驗，同埋少少關於 multiplayer 同 photon 嘅知識。
> 
> 其後又因為呢啲經驗，喺我畢業後又好彩有前老闆賞識，就帶咗我入Minidragon，參與到 mobile game 嘅營運同開發，而隻 game - Tiny Fantasy 當其時已經係有一歲，因為公司想擴張製作新遊戲所以請咗我去支撐呢隻 game。嗰個團隊裡面我就係做 Programmer 嘅職位啦，工作內容一半係負責製作新嘅角色技能、裝備技能，俾設計師塞返落個 prefab 入面用；一半係改進遊戲本身，有時係製作新嘅功能，有時係 fix bug 或者 profile 隻 game。
> 
> 另外呢，好多時都會直接同玩家溝通。因為隻 game 有 run 一個 discord community，啲玩家有咩想法呀、鍾唔鍾意新裝備新角色呀、中咗咩bug呀、有咩頭暈身慶呢，都會喺個 discord 度出聲。而公司呢，同埋我，都好注重儲 fans 呢樣嘢，所以都會擺心機同佢哋溝通交流。
> 
> 然後時移勢亦，我都喺間公司留咗一年多少少。而係一年裡面我不時都會感受到自己喺 Programming 同 Computer Science 上嘅知識淺薄嘅呢個短板。而我想要彌補呢啲不足，同解決呢啲問題，所以我後嚟辭咗職同讀咗少少 CS 嘅 online course。
> 
> 到依家我有信心可以處理到複雜少少嘅 Program 問題或者 project 問題喇，所以我就覺得可能係時候同啲職場人仕傾下偈，然後可以的話就希望搵返份工作。



舉個例子，我有次手多想改體力值方面嘅系統。原先隻 game 係喺玩家撳「開始遊戲」呢粒掣嘅時候，就喺 server 扣佢體力值。然後問題嚟喇，好多玩家會喺撳咗粒掣之後，熄game。可以有好多原因嘅：可以係卡loading，可以係佢以為卡loading，可以係佢忽然唔想打機。然後到佢哋重新開返遊戲就發現，明明我一鋪都冇玩過點解扣咗我體力呀？之後就嚟投訴喇。

所以嗰時係有啲呢個 debug life 嘅狀況發生緊。就係隻 game 本身有留低 bug，我工作上長期唔係 fix 緊 bug，就係趕緊新內容，結果就係個人嘅士氣會好低落。OT 完就冇剩低幾多心機去研究同實踐點樣去 refractor 份 project 去大改佢嘅 structure，而且要 refractor 又犠牲緊 fix bug 同整新內容嘅時間。另一樣嘢就係某程度 refractor 都唔及 fix bug 咁有價值係咪，refractor嘅成效唔係短期會見到，但係 fix bug 同 新內容 係直接影響緊 retention 同 income。所以我如是者過咗一段時間我就覺得，okay 我應該要出走去解決咗呢啲問題去，然後呢世唔好再踩呢個潭。



**工作經驗**
1. Multimedia → Part-time → multi-player
2. Minidragon → Tiny Fantasy → 技能製作、遊戲功能
3. Discord → 玩家交流 → 儲 fans
4. 一年 → CS 短板 → eg. dependency
5. debug life → 士氣低落 → refractor < fix bug
6. 進步解決問題 → 搵工
