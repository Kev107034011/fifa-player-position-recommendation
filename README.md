# 以 ML-KNN 結合 Tree-Structured Parzen Estimator 與 Grid Search 建構球員配置最佳化之輔助決策系統
>在考量球隊預算上限、球員潛力、薪資、能力等條件的前提下，輔助球隊進行最佳化球員配置，並建置一套輔助決策系統，提升決策品質。

足球轉會 (Transfer) 係指足球俱樂部間交易球員的活動，其發生原因繁雜，包含球隊戰略配置所需特定人才、球隊年度經費存在限制、現有球員配置不符經濟效益等等。
而在不同的決策目標與限制式的條件下，便衍生出了球隊球員配置的決策問題。

All data comes from FIFA 22 complete player dataset:  
https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset/code

## 整體分為兩大步驟進行
1. 以球員現有能力資訊與其各隊常鎮守之守備位置作基底，建構 ML-KNN 預測模型並透過 Tree-Structured Parzen Estimator(TPE) 優化其超參數，以此流程萃取各隊伍教練之經驗，找出各球員最適合的守備位置。
2. 連結選定目標之限制式鎖定搜索空間，如：成本考量、缺乏球員類型，再透過 Grid Search 從該目標空間中，選出球員配置的最佳決策。

## 定義問題屬性為多標籤分類問題 (Multi-label Classification)
- 單一球員於球隊中可能被部屬於多種位置
- 各隊教練依據經驗與球員各項能力素質評比，選擇適合之位置

## 使用 ML-KNN 去建構預測模型，因其貝氏推論過程有解釋力強與存在決策者可彈性調整之空間。



