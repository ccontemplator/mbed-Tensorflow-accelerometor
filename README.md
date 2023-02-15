# 
首先PYTHON端s.write(bytes("/accelerator_capture_mode/run\r", 'UTF-8')) 這個rpcfunction會開啟accelerator_capture_mode而accelerator_capture_mode會開啟thread來跑capture_function

capture_function 首先要先測量reference vector(後面 "change of direction"要用的)，之後測量5筆手勢，每一筆手勢我只取15個向量，並算出與reference vector的角度，算這五筆個別的15個向量與reference vector 呈大於三十度角則(我用一個 over[5]來存個別超過的數量)over[0~4]++，同時取的gesture index(id) 並publish id 與 個別的15個向量，最後再pubish over[]  (到此共15筆資料publish 到broker)
但似乎publish message中的sprintf會導致isr的問題。

python 收到15筆資料後(capture function做完)，則python 傳送rpc command s.write(bytes("/stop_capture/run\r", 'UTF-8')) 來join() capture_function 的thread 

最後是繪圖...
