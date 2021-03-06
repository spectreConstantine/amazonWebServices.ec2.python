## amazonWebServices.ec2.python
**AWS (Amazon Web Services) ec2 instances start and stop (running and stopping) by python**

- [x] --- 繁體中文版 ---

    此程的功能為提供使用 Amazon Web Service (AWS) 的 EC2 服務的使用者一個簡便的使用者界面
    能夠將每台EC2 Instance(虛擬機器)用Instance ID做成設定檔在使用時可以快速方便地開或/關機
    方法是在%userprofile%\.aws\目錄建立一個客製的文字檔其副檔名為.aws, 例如 username1.aws
    同時也用AWS CLI執行AWS Configure以建立%userprofile%\.aws\credentials提供程式存取權限

- [x] --- English Version ---

    The program is intended to provide users who are using EC2 service in AWS (
    Amazon Web Service) to instantly interact with the EC2 instances with a GUI (
    graphical user interface) by instance IDs in a pre-configured text-formatted file.
    To run the program, the AWS credentail should be configured by running AWS CLI. 
    With that, a %userprofile%\.aws\credentials file will be created. Additionaly, 
    you create each of the EC2 instances you have with a profile in %userprofile%\.aws
    folder by setting their credential profile name, instance ID, region and password.

    ![關機狀態](https://github.com/spectreConstantine/amazonWebServices.ec2.python/blob/master/2020-04-27_094454.png)


- [x] --- 程式使用說明如下 ---

    * 參考 執行環境需求 安裝 Python3 及 boto 套件。
    * 此程式需有一個自己客製的文字檔, 放在 %userprofile%/.aws/裡。
    * 取個名字, 副檔名需為 .aws, 比如username1.aws, 那就是 %userprofile%/.aws/username1.aws。
    * 然後在主程式會自動去%userprofile%/.aws/裡找所有副檔名為.aws的檔案. 程式如果只找到1個就直接打開。
    * 客製的文字檔中, 如果 SSH 中, Password放的是 PEM 的路徑, 但RDP中, Password放的是 PEM 已解開的密碼。
    * 如果使用 ssh 連線, 設定檔裡的 "Password" 需設定 .pem 的檔案位置, 例如 (* 注意, 使用 ssh 時 Password 為 .pem 的檔案位置)
      * 設定檔格式 ssh 為: {"Profile":"profile username", "Region":"ap-east-2", "ID":"i-01abc234defg5h6j7", "Password":"C://Users//username//.awscredentials//Ubuntu.pem"}')
    * 如果使用 rdp 連線, 設定檔裡的 "Password" 需則在 aws console 裡使用 .pem 將密碼解密為明碼後寫在設定檔裡 
      * 設定檔格式 rdp 為:{"Profile":"profile username", "Region":"ap-east-2", "ID":"i-01abc234defg5h6j7", "Password":"@?b!)axcyNKi1SqICn9oSygPx8k(Zm1e"}')             
    * 同時, 要用aws cli設定 AWS Configure, 這樣會在 %userprofile%/.aws/產生 credentials這個檔。
      * [profile username]
      * aws_access_key_id = AKcccccccxxxxxxxx4I
      * aws_secret_access_key = nxxx88xxxxxxud2KAxm
    * 如果有多個 profile 程式會選用不同的 credential。因此在你的檔案裡的profile和credential的profile名稱要對應。

- [x] --- 摘要幾個修改 ---

    * 設定檔改為需要加上 .aws 副檔名. 之前的版本就是沒有 .aws, 這樣是為了區隔所有放在 %userprofile%/.aws/ 的其他的檔案。
    * 像是如果有多台機器, 可以在 .aws 目錄下新增多個 .aws 檔案. 程式如果只找到1個就直接打開。
    * 如果有兩個以上會有一個簡單的選單. 輸入0~n按下enter. 如果沒輸入按enter, 預設是第1個(index=0)。
    * 如果Password欄位是.pem結尾, 就以ssh開啟. 否則用rdp開啟。
    * 程式的 title改為檔案的profile檔名(.aws前面)加上/credential的profile名稱, 這樣應該比較好識別。
    * 開關機按鈕按下去會有計數器, 開關機按鈕按下後, 每 2 秒鐘 (2000 ms) 會在更新伺服器狀態及次計數器計時。
    * 狀態列除了開機 running, 關機 stopped, 外, 其他的狀態像 pending 或 stopping 也都會在目前狀態中顯示。
    * 開關按鈕, 連線按鈕按下去後即彈起來. 連線部份也是跑在另一個thread. 不再會像當機一樣卡住。
    * 連線時的ssh/rdp指令會存成一個%userprofile%/.aws/executedCmd.(.txt檔案.有需要時打開來用(像在其他軟體putty裡)。


- [x] --- 執行環境需求 ---

    * 電腦裡要安裝Python 3.7.7 (這是我用的版本, 其他版本也許可以跑但沒實測過).
    * 同時要安裝 boto3 (使用命令列 pip install boto3), 其他都是內建的. 
    * 用python -OO -m py_compile Miranda_Ubuntu_aws_sshrdp_UIv7.py 可以編譯成.pyc (跑完後在__pycache__目錄下, 可以更名, 可以移到桌面跑).

- [x] --- 待新增功能/已知問題 ---    

    * 應該要將按鈕按下後鎖定。目前是如果你要開機，按下後會開機。但如果連續按了兩次，它會開機後再關機。目前只能記得如果按下去還沒即時反應，不要再重複一次。
    * 應要有一個更新狀態按鈕。目前是如果你在其他地方，例如 aws cli/aws console/aws mobile app/etc. 上更改了機器狀態，程式不會一直在界面上更新狀態。
    * 應該要開啟程式時更新狀態直到完成開/關機。如果你在開關機過程啟動程式，它只會顯示目前狀態。開/關機按鈕不會更新狀態。目前只能關掉程式再開一次。
    * 預計會再新增一個顯示目前 security group 清單，並能修改/刪除/新增的功能。這是由於通常會鎖定 custom IP，如果換了 IP 就需要再在 aws 界面 console 裡改。
