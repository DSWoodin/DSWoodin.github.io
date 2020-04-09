---
layout: post
title: Credit scorecard chapter 3
subtitle: Evaluation
tags: [credit scorecard]
---

Ở bài trước chúng ta đã có thể xây dựng được mô hình scorecard rồi. Nhưng như vậy là chưa đủ, chúng ta cần trình bày, diễn giải nó một cách khoa học, và dễ hiểu để thuyết phục công ty đưa mô hình vào vận hành.

Do vậy bài này mình sẽ nói về 1 số cách để diễn giải và trình mô hình cho sát với vận hành, với một số đầu mối trong bài các bạn hoàn toàn có thể tự build cho mình các report thật pro. 

Bài này kiến thức thì không có gì cao siêu nhưng hầu hết là do mình tự nghiền ngẫm và học hỏi. Do vậy rất mong nó có ích cho cách bạn dù ít hay nhiều.

Mục tiêu gồm :

    1. Giới thiệu các tiêu chuẩn đánh giá mô hình
    2. Các cách triển khai mô hình 


![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_intro.png)





## Đầu tiên build lại mô hình giống kỳ trước


Trong bài này sẽ đề cập đến cách diễn giải và trình bày là chính, do vậy ta sẽ dùng mô hình LogisticRegression để làm ví dụ cho đơn giản




## Dự báo với mô hình


**Dự đoán với phương thức predict():**

phương thức predict cho ta kết quả đầu ra với nhãn cụ thể :

    array([0., 0., 0., ..., 0., 0., 0.])

Một cách dễ hiểu nếu kết quả là :
 - 0 tương ứng với khách hàng bình thường
 - 1 tương ứng với khách hàng mục tiêu

    

Phân bổ của các lớp thực tế:



![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_0.png)




Phân bổ của các lớp theo dự báo:



![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_1.png)



Nhìn vào pie chart ta thấy được tỷ lệ giữa 2 class 0, 1 trong kết quả dự báo của tệp test có sự khác biệt so với thực tế. 

Có vẻ như mô hình dự báo chưa chính xác. Nhưng làm sao để biết được mô hinh chúng ta có hiệu quả và hiệu quả đến bao nhiêu. Chúng ta có thể đánh giá mô hình theo một số phương pháp dưới đây


# Các phương pháp đánh giá mô hình:


Trong các bài toán phân lớp nhị phân (chỉ có 2 lớp) có một lớp quan trọng hơn và cần dự báo chính xác thì lớp quan trọng hơn sẽ được gán nhãn là 1  **Positive**, biến còn lại là 0: **Negative**



Chúng ta cần làm quen với các khái niệm :

- **TP** - True Positive: Số lượng dự đoán là 1 và thực tế là 1 
- **FP** - False Positive: Số lượng dự đoán là 1 và thực tế là 1 
- **TN** - True Negative: Số lượng dự đoán là 0 và thực tế là 0 
- **FN** - False Negative: Số lượng dự đoán là 0 và thực tế là 1 
- **FPR** - False Positive Rate: Tỷ lệ dự báo sai biến Positive
- **FNR** - False Negative Rate: Tỷ lệ bỏ sót biến Positive

-**confusion_matrix**: thể hiện có bao nhiêu dữ liệu được dự đoán vào class nào và class thực sự của chúng.



![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_confusion.png)



**Accuracy:** = TP+TN / TP+TN+FP+FN  
    (Tỷ lệ dự đoán đúng )

**Recall:** = TP / TP+FN  
    (Tỉ lệ Dự đoán true positive trong số những điểm thực sự là positive (TP + FN).)

**Precision:** = TP / TP+FP  
    (Tỷ lệ số điểm true positive trong số những điểm được phân loại là positive (TP + FP))



#### confusion_matrix:



![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_2.png)




#### Các chỉ số:



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>metrix</th>
      <th>accuracy</th>
      <th>recall</th>
      <th>precision</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>score</th>
      <td>0.812</td>
      <td>0.265501</td>
      <td>0.620818</td>
    </tr>
  </tbody>
</table>
</div>



Quay lại một chút về vấn đề dự đoán với phương thức predict() để hiểu hơn tại sao mô hình lại có thể dự báo nhãn cho tệp test. 




![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_3.png)




Giá trị đầu ra của hàm sigmoid luôn nằm trong khoẳng từ 0 đến 1 nếu ta lấy ngưỡng(threshold) là 0.5 và chia dữ liệu làm 2 phần 
phần < 0.5 sẽ được gán nhãn là 0 và ngược lại thì ta sẽ được kết quả như phương thức predict() đã làm.

Phương thức predict_proba() trả về cho ta giá trị xác xuất, phương thức predict() thì lấy ngưỡng mặc định là 0.5 và đưa ra kết quả phân lớp.

Do vậy nếu ta sử dụng phương thức predict_proba() và thay đổi ngưỡng phân lớp thì kết quả phân lớp sẽ khác biệt rất nhiều, confusion_matrix, accuracy, recall, precision cũng sẽ thay đổi theo. 

Vì mục việc tìm ra các khách hàng không thanh toán - label 1 là quan trọng hơn nên chung ta sẽ cố gắng tăng **FPR** để giảm **FNR**, với tôn chỉ là thà giết lầm còn hơn bỏ sót. Bằng cách giảm ngưỡng(threshold) xuống dưới 0.5(vd: 0.2) chúng ta sẽ làm tăng lượng dữ liệu được gán nhãn **Positive** lên và **TP**, **FP** sẽ tăng , **TN**, **FN** sẽ giảm theo - đồng nghĩa với giá trị **FPR** sẽ tăng

Receiver operating characteristic - ROC curve sẽ thể hiện được FPR và TPR thay đổi thế nào qua các threshold thư viện sklearn 
hỗ trợ chúng ta khảo sát và vẽ ROC curve rất tốt.



Vẽ ROC curve và thể hiện 3 ví dụ trên biểu đồ:

    threshold:0.45, FPR:0.06, TPR:0.31
    threshold:0.13, FPR:0.45, TPR:0.75
    threshold:0.07, FPR:0.85, TPR:0.97



![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_4.png)



**AUC** - Area Under the Curve là diện tích nằm dưới ROC curve cũng là một tiêu chuẩn đánh giá mô hình, giá trị này càng cao thì mô hình càng tốt. 

Nếu thuộc lĩnh vực tài chính thì các bạn sẽ không lạ gì với chỉ số **GINI** - tiêu chuẩn để đo độ chính xác của mô hình credit scorecard.

Và chúng ta có thể tính GINI theo công thức đơn giản dưới đây:

**GINI = AUC * 2 - 1**


# Các phương pháp triển khai mô hình:

Sau khi chúng ta xây dựng được mô hình thì vấn đề triển khai thế nào cho dễ dàng hỗ trợ cho vận hành tốt nhất cũng là vấn đề quan trọng


### Bước 1:  Chuyển dải xác xuất về score:

Đầu tiên khi Chúng ta dùng phương thức predict_proba() để chấm điểm thì kết quả trả về sẽ là một dải xác xuất, ta hoàn toàn có thể dùng dải xác xuất này là điểm số để đánh giá chất lượng của từng trường hợp. 

Tuy nhiên con số xác xuất thì sẽ không được mỹ quan và thân thiện với các bộ phận vận hành vì vậy chúng ta có thể chuển dải xác xuất về dải điểm từ 0 đến 1000

Dữ liệu thường có phân phối lệch trái do số giá trị có class 0 chiếm áp đảo, cho nên điều quan trọng hơn hết là khi ta chuyển dải xác xuất sang dải score ta có thể tổ chức lại phân phối của dải điểm để gần với phân phối chuẩn.




![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_5.png)



**Phân phối của dải score cân xứng hơn hẳn, sẽ phù hợp cho ứng dụng, hỗ trợ vận hành hơn dải xác xuất.**




![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_6.png)




### Bước 2: Chuyển dải score về các bin:

Tuy dải score đã đẹp rồi, nhưng ta vẫn cần khái quát chúng hơn nữa để có thể dễ dàng theo dõi performance dễ dàng hơn. 

Trường hợp dưới đây mình gon dải score vào 5 bin sao cho số lượng phần tử mỗi bin xấp xỉ nhau, từ đó ta có thể đễ dàng phân tích theo nhiều chiều hơn.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total</th>
      <th>positive</th>
      <th>%positive</th>
      <th>interval</th>
    </tr>
    <tr>
      <th>bin</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1.0</th>
      <td>619</td>
      <td>47.0</td>
      <td>7.592892</td>
      <td>(366.99999, 458.0]</td>
    </tr>
    <tr>
      <th>2.0</th>
      <td>595</td>
      <td>72.0</td>
      <td>12.100840</td>
      <td>(458.0, 484.0]</td>
    </tr>
    <tr>
      <th>3.0</th>
      <td>587</td>
      <td>86.0</td>
      <td>14.650767</td>
      <td>(484.0, 507.0]</td>
    </tr>
    <tr>
      <th>4.0</th>
      <td>602</td>
      <td>136.0</td>
      <td>22.591362</td>
      <td>(507.0, 553.0]</td>
    </tr>
    <tr>
      <th>5.0</th>
      <td>597</td>
      <td>288.0</td>
      <td>48.241206</td>
      <td>(553.0, 747.0]</td>
    </tr>
  </tbody>
</table>
</div>





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>score</th>
      <th>y</th>
      <th>bin</th>
      <th>bin_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2935</th>
      <td>470</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>(458.0, 484.0]</td>
    </tr>
    <tr>
      <th>3182</th>
      <td>459</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>(458.0, 484.0]</td>
    </tr>
    <tr>
      <th>2238</th>
      <td>466</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>(458.0, 484.0]</td>
    </tr>
    <tr>
      <th>4786</th>
      <td>511</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>(507.0, 553.0]</td>
    </tr>
    <tr>
      <th>8048</th>
      <td>490</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>(484.0, 507.0]</td>
    </tr>
  </tbody>
</table>
</div>






![Crepe](https://raw.githubusercontent.com/minmax49/minmax49.github.io/master/img/scorecard_chapter3_7.png)



Với cách thức trên thì việc đưa mô hình vào vận hành là rất thuận tiện rồi. Bài kỳ này xin kết thúc tại đây. 

Kỳ sau mình sẽ nói về cách đánh giá lại hiệu quả ứng dụng mô hình và tối ưu vận hành qua mô hình nhân quả.

Mong các bạn đón xem.
