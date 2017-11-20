---
title: Infrastructure Names
tags: 去魅
graphviz: true
---
更了解公司的系統以後，又再度有種 "其實沒那麼難，就是這樣而已" 的感覺。
Infrastructure 說到底就是一堆機器，為了在管理這些機器時能夠用統一的概念和詞彙溝通，而挪用了一些既有的名詞湊成專有的名詞組。

{% include graphviz.html
   title="infrastructure_names"
   type="digraph"
   graph="rankdir=LR;
          RuntimeEnv -> Ecosystem;
          Ecosystem -> Habitat;
          Habitat -> Machine;
          SuperRegion -> Region;
          Region -> Habitat;"
 %}

當我們在使用這些名詞（指稱實際的 infrastructure）溝通時，通常有兩層概念需要區分：邏輯層，和地理層。
邏輯層：這台機器的主要任務是什麼？
地理層：這台機器在哪裡？

### 邏輯層：
* Runtime Environment 有三個： Dev, Stage, 和 Prod。
    *  其實小型專案，例如寫個部落格，也是在一台筆電上做這些事情。你需要一個環境來開發和測試（dev），一個環境是拿開發的結果實際上線跑跑看（stage），最後都確認沒問題了就要真的上線讓人使用（prod）。
    * 這些環境都是以 Prod 為準，複製 Prod 的環境之後，工程師再依據自己的需求設定。
    * 公司因為有八百個工程師要一起開發，提供的服務又遍及全世界，所以每個環境底下都有很多台機器（和虛擬機器），但是這些環境最終還是以上述流程來區分和管理。
* Ecosystem
    * 各個 RuntimeEnv 底下也會有不同的需求，例如：
    * 同樣在 Dev 裡面，除了給不同團隊的工程師開發不同服務的 DevA、DevB 之外，也有測試用的環境：TestA，TestB
    * Stage 下面也有 StageA、StageB
    * Prod 比較特殊，就只有一個 Prod
    * 這些每一個就都是一個 Ecosystem

### 地理層：
上面說的環境或生態系，其實都包含很多台機器，這些機器有時候會放在一起，因為放在一起有工程上的優勢：機器和機器之間溝通的速度比較快、一起管理比較省錢、備援方便，等等諸如此類的原因；有時候不會放在一起，因為放在離使用者（包含客戶和工程師）比較近的地方，機器提供服務的速度（主要取決於網路的速度和距離遠近）會比較快。

* Habitat：
    * 就是實際的 **Data Center，機房**。
    * 在雲時代，公司除了自己的機房外，也有使用 AWS 的雲。
    * 自己的一個機房或者 AWS 的一個 Availability Zone，我們都獨立稱為一個 Habitat。
    * 同在一個 Habitat 的機器有時候會屬於不同的 Ecosystem，例如：因為公司的工程團隊主要在舊金山，在矽谷也有很多我們服務的重度使用者，所以，AWS 的 US-West-1A 這個機房（habitat），就同時有 uswest1a-devc（屬於 DevC 這個 Ecosystem）和 uswest1a-prod（屬於 Prod 這個 Ecosystem）的機器。
    * Habitat 的命名實際上乘載了兩層意義，"-" 前半段是指機房所在的地理位置，後半段是指邏輯上的角色。

* Region：
    * 在同一個地理區域，有時候會有多個機房：例如 AWS 的機房在北加州就有兩個：（US-West-1A 和 US-West-1B），這樣做可以保證萬一一個機房停電，另一個機房還可以備援。
    * 不過這兩個機房還是相當靠近（至少比起和其他機房的距離），假如舊金山大地震，這兩個機房就還是有很大可能會同時被影響。
    * 這兩個機房彼此之間的網路距離也比較短（大概 <10ms）我們就可以把他們稱作同在一個 Region，再依據其邏輯上的功能，稱為 uswest1-devc。
    * 我們自己在舊金山也有兩個機房，SFO1 和 SFO2。同樣數於 DevC 的機器，就一起稱為 sfo12-devc

* Super Region：
    * 延續同樣的理由，整個北加州的所有機房，屬於開發用途的機器，就統稱為 norcal-devc，裡面就包含了 uswest1-devc 和 sfo12-devc 兩個 Region，和其下所屬的 uswest1a-devc，uswest1b-devc，sfo1-devc，和 sfo2-devc 這四個 Habitat.
