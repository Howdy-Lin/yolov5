# yolov5 (如何使用yolov5訓練自己的模型進行object detection)
## 環境:GPU RTX-3070,driver version:510.47.03,cuda:11.6<br>torch version:1.8.0+cu111,torchvision==0.9.0+cu111 torchaudio==0.8.0
## *操作前心得,踩過坑的點
在使用yolov5之前,我嘗試過yolov2及yolov4,但往往受環境影響無法成功<p>例如yolov2使用的darkflow程式較舊,無法套用在我環境下的tensorflow2<p>
  而yolov4使用上又相當麻煩,所以最後找到較為快速上手的yolov5,但仍遇到幾個坑點,以下逐一贅述<p>
 (1)標注問題,要注意text檔打開來是否正確,最左邊的數字不可以小於零
      <p align="center"><img src="https://user-images.githubusercontent.com/74965449/158050109-ac5400e1-6519-4a3c-bdb6-1cb60450687c.png" width="90%" height="90%"></p>
(2) 注意所有cuda,cudnn版本是否匹配,若gpu無法啟用成功 可能是pytorch版本問題<p>
可以參考[pytorch官網](https://pytorch.org/get-started/previous-versions/)找尋符合cuda版本的torch
## (1) 第一步驟
clone上yolov5官網github <br>
```bash
git clone https://github.com/ultralytics/yolov5 
cd yolov5
pip install -r requirements.txt  
```
## (2) 蒐集測試集與驗證集
在這使用[makesense.ai](https://www.makesense.ai/) 進行資料標注, 存成.txt檔
儲存在yolov5的位置下  如下圖:
<p align="center"><img src="https://user-images.githubusercontent.com/74965449/158048619-cbb33597-f447-4244-a0ee-2e58b07e918f.png" width="50%" height="50%"></p>

## (3) 修改符合自己需求的yaml檔
依照自身需求改nc 及 names<br>
<p align="center"><img src="https://user-images.githubusercontent.com/74965449/158048791-662f452f-c531-4e6b-b96d-7d671ecf3493.png" width="70%" height="70%"></p>

## (4) 執行結果視覺化
pip安裝Weights & Biases (W&B) 套件 讓訓練過程及結果視覺化
```bash
 pip install wandb

```
## (5) 執行train.py
cd到yolov5目錄 並執行以下程式碼
```bash
# Train YOLOv5s on custom_data for 120 epochs
$ python train.py --img 640 --batch 8 --epochs 120 --data custom_data.yaml --weights yolov5s.pt
```
## (6)得訓練結果
<p align="center"><img src="https://user-images.githubusercontent.com/74965449/158049339-0b18ebf7-0bb6-4ef7-a6a1-dd880d5ef483.png"></p>
訓練結果的.pt檔會存在train/weights資料夾內<p>
  測試圖片cd到yolov5目錄內執行以下命令
  
```bash
  python detect.py --weights runs/train/exp1/weights/last.pt --img 640 --conf 0.4 --source data/images
  ```
測試結果範例<p>
衛生紙可以準確辨識出來
  <p align="center"><img src="https://user-images.githubusercontent.com/74965449/158049745-b2950c2d-b670-4858-9938-fb85e916b2dd.png" width="30%" height="30%"></p>
衛生紙以外的東西無法辨識
  <p align="center"><img src="https://user-images.githubusercontent.com/74965449/158049855-5258718a-6d0c-46ed-8464-277830c61914.png" width="30%" height="30%"></p>
   <p align="center"><img src="https://user-images.githubusercontent.com/74965449/158049845-fe77b545-1bef-4550-b013-b227ab03b82e.png" width="30%" height="30%"></p>
## 參考資料
https://github.com/ultralytics/yolov5
