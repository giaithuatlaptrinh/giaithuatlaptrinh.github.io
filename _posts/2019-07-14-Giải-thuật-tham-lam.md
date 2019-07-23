# Bài 7: Tham lam

[comment]: <> 

Tham lam là một trong những phương pháp phổ biến nhất để thiết kế giải thuật. Tham lam thường là thuật toán dạng lặp, trong đó tại mỗi bước, ta xây dựng lời giải dần dần, cho đến khi thuật toán lặp kết thúc ta sẽ thu được lời giải cuối cùng của bài toán.  Ý tưởng của tham lam, như cái tên đã gợi ý cho ta, là:


<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><span style="color:dodgerblue"> Nguyên lý tham lam: </span> Tại mỗi bước của thuật toán, trong số các lựa chọn khả  , chọn một lựa chọn có lợi nhất.</div>
<br/>

Rất nhiều thuật toán nổi tiếng được thiết kế dựa trên tư tưởng của tham lam, ví dụ như thuật toán tìm đường đi ngắn nhất của Dijkstra, thuật toán cây khung nhỏ nhất của Kruskal, v.v. Trong bài này chúng ta sẽ tìm hiểu nguyên lý thiết kế tham lam thông qua một vài ví dụ.

# 1. Lưu file trên đĩa từ


Bài toán như sau:
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <span style="color:dodgerblue">  Bài toán 1</span>:  Giả sử bạn có $n$ file trên đĩa từ trong đó file thứ $i$ có dung lượng $L[i]$. Gọi $\pi$ là một hoán vị của $\{1,2,\ldots,n\}$ tương ứng với một cách lưu trữ file theo thứ tự $\pi(1),\pi(2), \ldots, \pi(n)$. Để truy cập file $\pi(i)$, bạn phải duyệt qua tất cả các file $\pi(1),\pi(2),\ldots, \pi(i-1)$. Do đó chi phí để truy cập file $\pi(i)$ là:
<p style="text-align: center;"> $C(\pi(i)) = \sum_{i=1}^{k} L[\pi(k)] $</p>
Tìm cách lưu trữ file sao cho việc truy xuất được hiệu quả nhất, biết rằng mỗi file được truy cập <b>đúng 1 lần</b>.
</div>

> **Ví dụ 1:** Giả sử các file đánh số $1,2,3$ có dung lượng lần lượt là $5,4,6$. Nếu ta sắp xếp file theo thứ tự ${2,3,1}$ thì chi phí truy nhập là $4+10+15 = 29$. Nếu ta sắp theo thứ tự $2,1,3$ thì chi phí truy nhập là $4+9+15 =28$.


Ý tưởng của giải thuật tham lam như sau: giả sử ta đang sắp xếp file vào vị trí thứ $i$, để giảm chi phí truy nhập file thứ $i$, ta nên lưu trữ các vị trí $1,2,\ldots i-1$ bằng các file với tổng dung lượng nhỏ nhất. Cách lưu nào sẽ thoả mãn tính chất này với mọi $i$? Đó chính là lưu các file theo thứ tự từ nhỏ đến lớn theo dung lượng. Trong ví dụ 1, cách lưu $2,1,3$ có chi phí nhỏ hơn là vì nó là cách lưu theo thứ tự từ nhỏ đến lơn. Ta có giả mã như sau:

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> GreedyFileOnTape</u>($L[1,2,\ldots, n]$):<br/>
&nbsp;&nbsp;&nbsp; $S \leftarrow \{1,2,\ldots,n\}$<br/>
&nbsp;&nbsp;&nbsp; <b>repeat</b><br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; choose $s \in S$ with minimum $L[s]$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; write $s$ to the tape<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $S \leftarrow S \setminus \{s\}$<br/>
&nbsp;&nbsp;&nbsp; <b>until</b> $S = \emptyset$<br/>
</div>
&nbsp;


**Tính đúng đắn của thuật toán**

Ta sẽ chứng minh cách lưu file theo thứ tự từ nhỏ đến lớn có chi phí nhỏ nhất. Giả sử tồn tại một cách lưu trữ tối ưu $\pi$ và chỉ số $i$ sao cho $L[\pi(i)] &gt; L[\pi(i+1)]$. Gọi $cost_{\pi}$ là chi phí truy nhập của $\pi$. Theo định nghĩa, ta có: $cost_{\pi} = \sum_{i=1}^n C(\pi(i))$.

Gọi $\pi'$ là hoán vị thu được từ $\pi$ bằng cách đổi chỗ $\pi(i)]$ và $\pi(i+1)$. Ta có:
<p style="text-align: center;"> $\begin{array} {lcl} cost_{\pi} - cost_{\pi'} &amp; = &amp; C(\pi(i)) + C(\pi(i+1)) - C(\pi'(i)) - C(\pi'(i+1)) \\&amp; = &amp; L[\pi(i)] - L[\pi(i+1)] <  0 \end{array}$</p>
Do đó, $cost_{\pi} > cost_{\pi'}$, trái với giả thiết $\pi(i)$ là cách lưu trữ tối ưu.

<hr width = "20%">
**Phân tích thời gian**

Bằng cách thực hiện sắp xếp theo chiều tăng của kích thước file, chúng ta có thể thực thi thuật toán trên trong thời gian $O(n\log n)$. 
<hr width = "20%">



Bây giờ chúng ta hãy thay đổi bài toán một chút như sau:

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <span style="color:dodgerblue">  Bài toán 2</span>: Mọi thiết lập vẫn như bài toán 1, tuy nhiên bây giờ file thứ $i$ có tần số truy cập là $F[i]$. Do đó chi phí để truy cập file $\pi(i)$ bây giờ là là:
<p style="text-align: center;"> $C(\pi(i)) =F[\pi(i)] \sum_{i=1}^{k} L[\pi(k)] $</p>
Tìm cách lưu trữ file sao cho việc truy xuất được hiệu quả nhất.
</div>
&nbsp;

Với bài toán này, tổng chi phí để truy xuất file là:
<p style="text-align: center;"> $Cost_{\pi} = \sum_{i=1}^n F[\pi(i)] \sum_{k=1}^iL[\pi(k)]$</p>

> **Ví dụ 2:** Giả sử các file đánh số $1,2,3$ có dung lượng lần lượt là $5,4,6$ với tần suất truy nhập là $2,1,1$. Nếu ta sắp xếp file theo thứ tự ${2,3,1}$ thì chi phí truy nhập là $4+10+2\times15 = 44$. Nếu ta sắp theo thứ tự $2,1,3$ thì chi phí truy nhập là $4+2\times 9+15 =37$.

Với bài toán tổng quát, trong ví dụ 2, ta vẫn thấy sắp xếp theo chiều tăng dần dung lượng có chi phí truy nhập rẻ hơn. Liệu đây có phải là cách tối ưu? Hay câu hỏi quan trọng hơn và tổng quát hơn:

> **Câu hỏi 1**: khi tần số khác nhau, liệu sắp xếp file theo thứ tự tăng dần có phải là cách sắp xếp tối ưu?

> **Trả lời**: Không. Xét hai file đánh số $1,2$ có dung lượng lần lượt là $1,2$ và tần số truy nhập là $1,10$. Nếu sắp xếp theo thứ tự $1,2$, thì chi phí truy nhập là $1 + 10\times (1+2) = 31$, trong khi sấp xếp theo thứ tự $2,1$ có chi phí truy nhập là $10\times 2 + 1 = 21$. 

Câu trả lời trên gợi ý cho ta ý tưởng sau: lưu file có tần số truy nhập lớn hơn trước. Tuy nhiên, ý tưởng này vẫn chưa chuẩn, vì nếu các file có tần số bằng nhau, thì ta sẽ muốn lưu file có kích thước nhỏ trước. Từ đây ta chỉnh lại ý tưởng như sau:

<p style="text-align: center;">  Lưu file theo tỉ lệ $L[i]/F[i]$ tăng dần.</p>

Trong câu trả lời cho câu hỏi 1, ta lưu theo thứ tự $2,1$ có chi phí thấp hơn vì tỉ lệ dung lượng / tần số của file 2 (là $2/10$) nhỏ hơn của file 1 (là $1/1$). Giả mã của thuật toán như sau:

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> GreedyFileOnTape2</u>($L[1,2,\ldots, n]$):<br/>
&nbsp;&nbsp;&nbsp; $S \leftarrow \{1,2,\ldots,n\}$<br/>
&nbsp;&nbsp;&nbsp; <b>repeat</b><br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; choose $s \in S$ with minimum $\frac{L[s]}{F[s]}$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; write $s$ to the tape<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $S \leftarrow S \setminus \{s\}$<br/>
&nbsp;&nbsp;&nbsp; <b>until</b> $S = \emptyset$<br/>
</div>
&nbsp;

**Tính đúng đắn của thuật toán**

Ta sử dụng ý tưởng tương tự trong trường hợp tần số bằng $1$. Giả sử tồn tại một cách lưu trữ tối ưu $\pi$ và chỉ số $i$ sao cho $\frac{L[\pi(i)]}{F[\pi(i)]} \leq \frac{L[\pi(i+1)]}{F[\pi(i+1)]}$. Gọi $\pi'$ là hoán vị thu được từ $\pi$ bằng cách đổi chỗ $L[\pi(i)]$ và $L[\pi(i+1)]$. Ta có:
<p style="text-align: center;"> $\begin{array} {lcl} Cost_{\pi} - Cost_{\pi'} &amp; = &amp;  F[\pi(i)]C(\pi(i)) + F[\pi(i+1)]C(\pi(i+1)) \\ &amp;  &amp; - F[\pi'(i)]C(\pi'(i)) - F[\pi'(i+1)]C(\pi'(i+1))\\ &amp; = &amp; F[\pi(i+1)]L[\pi(i)] -  F[\pi(i)]L[\pi(i)] < 0 \end{array}$</p>
vì $\frac{L[\pi(i)]}{F[\pi(i)]} \leq \frac{L[\pi(i+1)]}{F[\pi(i+1)]}$.  Do đó, $Cost_{\pi} < Cost_{\pi'}$, trái với giả thiết $\pi(i)$ là cách lưu trữ tối ưu.

<hr width = "20%">

**Phân tích thời gian**

Bằng cách thực hiện sắp xếp theo chiều tăng của tỉ lệ kích thước / tần số, chúng ta có thể thực thi thuật toán trên trong thời gian $O(n\log n)$. 
<hr width = "20%">


# 2. Bài toán chọn môn học


<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <span style="color:dodgerblue">  Bài toán 3</span>: Trong kì này bạn muốn học $n$ môn học, trong đó tiết học của môn học thứ $i$ bắt đầu tại thời điểm $S[i]$ và kết thúc tại thời điểm $F[i]$. Bạn không được phép chọn hai môn học mà khoảng thời gian của hai môn học gối lên nhau. Làm thế nào để bạn có thể tìm được một tập lớn nhất các môn học không gối lên nhau.
</div>
&nbsp;

> **Ví dụ 2:** ở hình dưới đây (lấy từ [1]), mỗi môn học được biểu diễn bởi một thanh ngang có chiều dài $F[i]-S[i]$. Tập các thanh màu xanh chính là một tập có số lượng lớn nhất các môn học không gối lên nhau:

<img src="http://www.giaithuatlaptrinh.com/wp-content/uploads/2015/07/class-schedule.png" alt="class-schedule">

Ở đây ta có thể nghĩ đến 3 cách tham lam:
<ol>
<li>Chọn lớp học bắt đầu sớm nhất trước. Tuy nhiên bạn có thể nghĩ ngay đến vấn đề của các chọn này là nếu có một lớp học bắt đầu sớm nhất nhưng lại kéo dài rất lâu thì bạn chỉ có thể chọn được 1 lớp học. Trong phần bài tập bạn sẽ phải tìm một phản ví dụ cho ý tưởng này.</li>
<li>Chọn lớp học ngắn nhất trước. Vấn đề của các chọn này là nếu có một lớp học ngắn nhưng gối lên các lớp học bắt đầu trước và sau lớp học này, bạn sẽ không thể có phương án tối ưu.  Trong phần bài tập bạn sẽ phải tìm một phản ví dụ cho ý tưởng này.</li>
<li>Chọn lớp học có thời gian hoành thành sớm nhất trước. Có vẻ đây là một cách chọn tham lam tốt vì nếu chọn lớp hoàn thành sớm  nhất trước, bạn sẽ có nhiều thời gian còn lại để chọn các lớp học khác. Thực tế đây là một cách chọn tham lam cho bạn phương án tối ưu.</li>
</ol>
Giả mã của thuật toán như sau:
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> GreedyClassSelection</u>($S[1,2,\ldots, n], F[1,2,\ldots,n]$):<br/>
1.&nbsp;&nbsp;&nbsp; sort $F$ in increasing order<br/>
2.&nbsp;&nbsp;&nbsp; permute indices of $S$ to match $F$<br/>
3.&nbsp;&nbsp;&nbsp; $J \leftarrow \{1\}$<br/>
4.&nbsp;&nbsp;&nbsp; tmp $\leftarrow $1<br/>
5.&nbsp;&nbsp;&nbsp; <b>for</b> $i \leftarrow 2 $ to $n$<br/>
6.&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <b>if</b> $F[tmp] \leq S[i]$   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; <i>$\ll$ class $i$ does not confict with tmp $\gg$</i><br/>
7.&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; tmp $\leftarrow$ i<br/>
8.&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $J \leftarrow J \cup \{i\}$<br/>
9.&nbsp;&nbsp;&nbsp; return $J$<br/>
</div>
&nbsp;

**Tính đúng đắn của thuật toán**

Giả sử $X = \{x_1,x_2,\ldots, x_k\}$ là tập các lớp học đầu ra của thuật toán tham lam và $Y = \{y_1,y_2,\ldots, y_m \}$ là một phương án tối ưu ($k \leq m$), sắp xếp theo chiều tăng dần của thời gian hoành thành. Gọi $Y_k = \{y_1,y_2,\ldots,y_k\} \subseteq Y$ là tập $k$ phần tử đầu tiên của $Y$. Trong số các phương tối ưu, chọn phương án $Y$ sao cho $\|X \cap Y_k\|$ là lớn nhất. 

Nếu $X = Y_k$, ta có $m = k$ vì nếu không, giải thuật tham lam sẽ thêm ít nhất một lớp học nữa vào $X$. Do đó, $X$ là một phương án tối ưu.  Giả sử $X \not= Y_k$. Suy ra tồn tại $1 \leq  i \leq k$ sao cho $y_i \not= x_i$.  Ta chọn $i$ nhỏ nhất thỏa mãn $y_i \not= x_i$. Như vậy $y_j = x_j, 1 \leq j \leq i-1$. Do $x_i$ là lớp học có thời gian hoành thành sớm nhất sau khi $x_{i-1}$ đã hoàn thành, thời gian hoành thành của $x_i$ sớm hơn $y_i$. Suy ra $x_{i}$ kết thúc trước khi $y_{i+1}$ bắt đầu.  Không khó để chứng minh được $y_i \not\in X$ (xem phần bài tập).


Xét $Y' = Y \setminus \{y_i\} \cup \{x_i\}$. Cụ thể
<p style="text-align: center;"> $Y' = \{x_1,x_2,\ldots, x_{i-1}, x_i, y_{i+1}, \ldots, y_m\}$</p>
Dễ thấy, $|Y'| = Y = m$ và các lớp trong $Y'$ không gối lên nhau. Do đó $Y'$ là một phương án tối ưu. Tuy nhiên $|X \cap Y'_k| &gt; |X \cap Y_k|$, trái với cách chọn $Y$ là phương án có $|X \cap Y_k|$ lớn nhất. Do đó, $X$ là phương án tối ưu.
<hr width = "20%">

**Phân tích thời gian**

Dòng 1 của thuật toán có thời gian $O(n \log n)$. Dòng thứ 2 có thể thực hiện được trong thời gian $O(n)$. 
Bằng cách thực hiện sắp xếp theo chiều tăng của tỉ lệ kích thước / tần số, chúng ta có thể thực thi thuật toán trên trong thời gian $O(n\log n)$. Ta có thể dùng một danh sách liên kết để lưu trữ $J$, do đó dòng 5 đến 8 có thể thực hiện trong thời gian $O(n)$. Tóm lại, tổng thời gian của thuật toán là $O(n\log n)$.
<hr width = "20%">


Phương pháp chứng minh bằng phản chứng cho bài toán chọn lớp học cũng là một phương pháp chứng minh phổ biến để chứng minh thuật toán tham lam là tối ưu. Ta có thể rút ra các bước như sau:
<ol>
<li>Giả sử lời giải tham lam $X$ là không tối ưu. Chọn lời giải tối ưu $Y$ khác $X$ với $|Y\cap X|$ lớn nhất.</li>
<li>Xét hai phần tử khác nhau đầu tiên giữa hai lời giải này.</li>
<li>Chứng minh bằng bằng cách trao đổi hai phần tử khác nhau đầu tiên, ta thu được một lời giải tối ưu khác $Y'$ có $|X\cap Y| &gt; |X \cap Y|$, qua đó, ta thu được phản chứng.</li>
</ol>
Bạn có thể thử phương pháp trên cho bài toán lưu trữ file trên đĩa từ.



# 3. Tham khảo 

[1] Jeff Erickson. <a href="http://web.engr.illinois.edu/~jeffe/teaching/algorithms/notes/07-greedy.pdf" target="_blank">Greedy Algorithm Lecture Notes</a>, UIUC.<br/>
[2] George Kocur, MIT OWC <a href="http://ocw.mit.edu/courses/civil-and-environmental-engineering/1-204-computer-algorithms-in-systems-engineering-spring-2010/lecture-notes/MIT1_204S10_lec10.pdf" target="_blank">lecture notes</a>, MIT.<br/>
[3] Cormen, Thomas H.; Leiserson, Charles E., Rivest, Ronald L., Stein, Clifford.<i>Introduction to Algorithms (2nd ed.)</i> . MIT Press and McGraw-Hill (2001). ISBN 0-262-03293-7.<br/>



# 4. Bài tập

**Bài 1:** Tìm cách sắp xếp tối ưu cho tập các file trong ví dụ $2$.

**Bài 2:** Tìm phản ví dụ cho ý tưởng 1 trong phần 2, bài toán chọn môn học.

**Bài 3:** Tìm phản ví dụ cho ý tưởng 2 trong phần 2, bài toán chọn môn học.

**Bài 4:** Chứng minh rằng $y_i \not\in X$ trong chứng minh tính đúng đắn của thuật toán tham lam trong bài toán chọn môn học.

**Bài 5:** Thực thi cả hai thuật toán trong phần 1 và 2 bằng ngôn ngữ C.

**Bài 6:** Bạn được ông chủ giao cho $n$ tác vụ và một chiếc máy tính để thực hiện $n$ tác vụ đó. Tác vụ thứ $i$ có thời hạn $D[i]$ và nếu bạn hoành thành tác vụ đó trước thời hạn $D[i]$, bạn sẽ được thưởng $P[i]$ đồng, còn nếu hoành thành sau $D[i]$, bạn sẽ chẳng nhận được đồng tiền thưởng nào cả. Biết rằng mỗi tác vụ mất thời gian đúng 1 đơn vị để hoành thành. Tìm một cách thực thi các tác vụ sao cho bạn nhận được nhiều tiền thưởng nhất. Giả sử thời hạn của các tác vụ là trong khoảng $[1,n]$.

Ví dụ bạn có 5 tác vụ với thông tin như sau:
<table style="width:100%">
<tbody>
<tr>
<td><b>Job</b></td>
<td><b>Profit</b></td>
<td><b>Deadline</b></td>
</tr>
<tr>
<td>1</td>
<td>100</td>
<td>2</td>
</tr>
<tr>
<td>2</td>
<td>19</td>
<td>1</td>
</tr>
<tr>
<td>3</td>
<td>27</td>
<td>2</td>
</tr>
<tr>
<td>4</td>
<td>25</td>
<td>1</td>
</tr>
<tr>
<td>5</td>
<td>15</td>
<td>3</td>
</tr>
</tbody>
</table>
Phương án tối ưu là (theo trình tự thực thi) $\{1,3,5\}$ với tổng tiền thưởng thu được là 142.

<ol> <li>Thiết kế thuật toán tham lam giải bài toán trên.</li>
<li>Phân tích tính đúng đắn và thời gian chạy của thuật toán đã thiết kế.</li>
</ol>
