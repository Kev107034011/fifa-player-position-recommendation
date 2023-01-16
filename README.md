# fifa-player-position-recommendation
>在考量球隊預算上限、球員潛力、薪資、能力等條件的前提下，輔助球隊進行最佳化球員配置，並建置一套輔助決策系統，提升決策品質。

足球轉會 (Transfer) 係指足球俱樂部間交易球員的活動，其發生原因繁雜，包含球隊戰略配置所需特定人才、球隊年度經費存在限制、現有球員配置不符經濟效益等等。
而在不同的決策目標與限制式的條件下，便衍生出了球隊球員配置的決策問題。

All data comes from FIFA 22 complete player dataset:  
https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset/code

# 整體分為兩大步驟進行
1. 以球員現有能力資訊與其各隊常鎮守之守備位置作基底，建構預測模型以萃取各隊伍教練之經驗，找出各球員最適合的守備位置
2. 連結選定目標之限制式鎖定搜索目標空間，如：成本考量、缺乏球員類型，再透過 Grid Search 從該目標空間中，選出球員配置的最佳決策



