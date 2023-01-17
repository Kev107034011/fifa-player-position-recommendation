# 以 ML-KNN 結合 Tree-Structured Parzen Estimator 與 Grid Search 建構球員配置最佳化之輔助決策系統
>在考量球隊預算上限、球員潛力、薪資、能力等條件的前提下，輔助球隊進行最佳化球員配置，並建置一套輔助決策系統，提升決策品質。

足球轉會 (Transfer) 係指足球俱樂部間交易球員的活動，其發生原因繁雜，包含球隊戰略配置所需特定人才、球隊年度經費存在限制、現有球員配置不符經濟效益等等。
而在不同的決策目標與限制式的條件下，便衍生出了球隊球員配置的決策問題。

All data comes from FIFA 22 complete player dataset:  
https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset/code

## 整體分為以下兩大步驟
**1. 以球員現有能力資訊與其各隊常鎮守之守備位置作基底，建構 ML-KNN 預測模型並透過 Tree-Structured Parzen Estimator(TPE) 優化其超參數，以此流程萃取各隊伍教練之經驗，找出各球員最適合的守備位置。**
- 詳細程式碼可參考：[球員守備位置預測程式](https://github.com/Kev107034011/fifa-player-position-recommendation/blob/main/Bayesian_Recommendation_PlayerPosition.ipynb)
- 程式碼中使用之套件 skmultilearn 因版本太舊，需要將套件檔案中的 mlknn.py 原始碼的第 165 行手動改成 
self.knn_ = NearestNeighbors(n_neighbors=self.k).fit(X)，才可順利運行。

**2. 連結選定目標之限制式鎖定搜索空間，如：成本考量、缺乏球員類型，再透過 Grid Search 從該目標空間中，選出球員配置的最佳決策。**
- 詳細程式碼可參考：[考慮成本預算之球員守備位置最佳化程式](https://github.com/Kev107034011/fifa-player-position-recommendation/blob/main/Player_Suggestion.ipynb)

## 首先，定義問題屬性為多標籤分類問題 (Multi-label Classification)
- 單一球員於球隊中可能被部屬於多種位置。
- 各隊教練依據經驗與球員各項能力素質評比，選擇適合之位置。
- 故使用 ML-KNN 去建構預測模型，因其貝氏推論過程有解釋力強與存在決策者可彈性調整之空間。
![image](https://user-images.githubusercontent.com/77613396/212628477-426fb147-df18-4d12-b714-bd0d3a061931.png)

## 再者，結合 ML-KNN 與 TPE 最佳化演算法，建構最佳化 F1-Score 之預測模型，預測各球員最適之守備位置 (可能為多個推薦)
- ML-KNN 預測出結果為貝氏機率，預測機率高於一特定閾值後即選為預測結果，故可能同時**推薦多種結果**。
- 另外，ML-KNN 中之 KNN 亦需要**定義 K 之大小**，即同時考慮 K 個相鄰點作為參考。
- 以上兩決策變數將放入 TPE 最佳化演算法，並設定最小化預測球員守備值之 F1-Score 為 Objective Function。
- 另外，亦提供多目標優化於程式碼內，可依據使用者需求在 Pareto-Front 上選取 Precision 與 Recall 之最佳組合點
![image](https://user-images.githubusercontent.com/77613396/212934612-8ac02ce2-c47f-4ff7-964b-dbf2c07b9e9e.png)

## 最後，根據前述步驟之預測結果，依據個案目標推薦出符合成本預算且最優化之球員配置組合，可參考下圖簡易流程。
![image](https://user-images.githubusercontent.com/77613396/212627811-831ea002-e2df-41a5-9852-f74afce1d7ad.png)

## 個案研究
>在球員適合守備位置之預測部分，將本專案使用之方法與常用演算法(XGBoost、RandomForest、NaiveBayes 進行 Bemchmark 後，發現有顯著提升效益。
![image](https://user-images.githubusercontent.com/77613396/212937876-af263df3-4d5a-420f-8e5f-05f815a59a64.png)

>以英超曼聯俱樂部球隊為例，取該球隊所有前鋒球員之薪資總合作為球隊更新前鋒位置之整體預算，再以本研究的資料集內前鋒數據與成本考量做對球隊來說合理化的分配。

由下圖可知，本專案成功在成本限制式內，建構出整體表現提高 12.90% 之球隊配置。
(圖的上方為原先曼聯之前鋒配置，下方為本專案所推薦之前鋒配置)
![image](https://user-images.githubusercontent.com/77613396/212628965-04e44c2d-9b81-4cc5-a732-7cb82fd4722f.png)

## 未來方向與實務應用
本專案基於研究性質存在以下限制
- 球員對於俱樂部及所在國家沒有喜好程度之差異
- 所有球員於各俱樂部間可自由交易
- 球員皆無傷病問題

若決策者基於現實考量，如球員解約金、無轉會之可能......等等導致有所限制，
將可從本專案 Grid Search 部分，對目標空間給予限制式以達成。




