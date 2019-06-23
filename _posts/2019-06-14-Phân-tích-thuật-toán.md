# Bài 3: Phân tích thuật toán

[comment]: <> Bài này mình hướng đến các bạn mới bắt đầu học thuật toán với mục đích giúp các bạn làm quen với một số khái niệm như $O(.), o(.), \Omega(.), \Theta(.)$. Mình đã có <a href="http://www.giaithuatlaptrinh.com/?p=27">một bài riêng</a> định nghĩa các khái niệm này một cách hình thức. Bài này mình bổ sung các định nghĩa đó bằng những giải thích trực quan hơn.

Khi nói đến một thuật toán người ta thường quan tâm đến hai câu hỏi: (1) thuật toán có dừng hay không và (2) thuật toán tiêu tốn bao nhiều tài nguyên tính toán. Câu hỏi (1) thường không khó để trả lời (nhưng không phải lúc nào cũng dễ). Câu hỏi (2) yêu cầu định nghĩa chi tiết thế nào là tài nguyên tính toán. Trong bài này, ta sẽ tìm hiểu sơ lược cách thức trả lời cả hai câu hỏi.

# 1. Tính dừng của thuật toán

Khi phân tích một thuật toán cho trước $A$, câu hỏi đầu tiên ta phải hỏi đó là liệu thuật toán có dừng trên mọi đầu vào hay không? Để trả lời câu hỏi này, thông thường ta có hai cách, tuỳ thuộc vào bản chất của thuật toán.

## 1.1. Thuật toán lặp

Trong dạng thuật toán này, người ta thường tìm một độ đo ''tiến độ'' của thuật toán, và biện luận rằng thuật toán luôn ''tiến bộ'' dựa trên độ đo này. Điều đó có nghĩa là thuật toán sẽ đạt được ''tiến độ'' cực tiểu (hoặc cực đại) sau một số hữu hạn bước, do đó, nó luôn dừng. Thông thường không khó để tìm ra một độ đo ''tiến độ''. Ta xét các ví dụ minh hoạ sau:

> **Ví dụ 1**: Thuật toán sau có dừng hay không?
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <u style="font: normal verdana; font-variant: small-caps"> SumOfArray</u>($A[1,2,\ldots, n]$):<br/>
&nbsp;&nbsp;&nbsp; $s \leftarrow 0$<br/>
&nbsp;&nbsp;&nbsp; <b>for</b> $i \leftarrow 1$ to $n$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; $s \leftarrow s + A[i]$<br/>
&nbsp;&nbsp;&nbsp; return $s$<br/>
</div>
<br/>
Dễ thấy thuật toán này lấy đầu vào là một mảng có $n$ phần tử và trả về tổng các phần tử trong mảng đó. Bạn có thể định nghĩa độ đo ''tiến độ'' chính là giá trị của biến chạy $i$. Mỗi bước của thuật toán (cụ thể là mỗi vòng lặp **for**), $i$ đều tăng lên 1. Do đó, thuật toán luôn tiến bộ ($i$ tăng lên 1). Do độ đo tiến độ cực đại có giá trị $n$, thuật toán phải dừng sau hữu hạn bước (cụ thể hơn là sau $n$ bước lặp).

Tuy nhiên, không phải lúc nào ta cũng có thể dễ dàng tìm đươc độ đo tiến độ. Xét ví dụ sau:

> **Ví dụ 2**: Thuật toán sau có dừng hay không?
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <u style="font: normal verdana; font-variant: small-caps"> Collatz</u>($n$):<br/>
&nbsp;&nbsp;&nbsp; $i \leftarrow n$<br/>
&nbsp;&nbsp;&nbsp; <b>while</b> $i > 1$ <br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <b>if</b> $i$ is odd <br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; $i \leftarrow 3i+1$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <b>otherwise</b> <br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; $i \leftarrow i/2$<br/>
</div>
<br/>
Theo bạn độ đo tiến độ của thuật toán trong ví dụ 2 là gì? Trên thực tế, cho đến hiện tại, người ta vẫn chưa biết thuật toán trên [có dừng](https://en.wikipedia.org/wiki/Collatz_conjecture) với mọi đầu vào $n$ hay không.

## 1.2. Thuật toán đệ quy

Đối với thuật toán đệ quy, tính dừng thường dễ dàng chứng minh được bằng phương pháp quy nạp.

> **Ví dụ 3**: Thuật toán sau có dừng hay không, giả sử đầu vào $n$ luôn dương.


<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> Fibonacci</u>($n$):<br/>
&nbsp;&nbsp;&nbsp;  <b>if </b>$n = 1$ or $n = 2$<br/>
&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;  return $1$<br/>
&nbsp;&nbsp;&nbsp; return <span style="font: normal verdana; font-variant: small-caps"> Fibonacci</span>($n-1$) + <span style="font: normal verdana; font-variant: small-caps"> Fibonacci</span>($n-2$)<br/>
</div>
<br/>
Dễ thấy khi $n = 1$ hoặc bằng $2$, lời gọi <span style="font: normal verdana; font-variant: small-caps"> Fibonacci</span>($n$) sẽ dừng. Do đó, ta có thể giả sử theo phương pháp quy nạp rằng <span style="font: normal verdana; font-variant: small-caps"> Fibonacci</span>($n-1$) và <span style="font: normal verdana; font-variant: small-caps"> Fibonacci</span>($n-2$) sẽ dừng. Như vậy <span style="font: normal verdana; font-variant: small-caps"> Fibonacci</span>($n$) cũng sẽ dừng vì thủ tục này chỉ phụ thuộc vào   <span style="font: normal verdana; font-variant: small-caps"> Fibonacci</span>($n-1$) và <span style="font: normal verdana; font-variant: small-caps"> Fibonacci</span>($n-2$).  

Nếu ta chỉ sửa đổi thuật toán trên một chút như dưới đây, sẽ tồn tại đầu vào khiến thuật toán sẽ không dừng, kể cả khi có điều kiện $n$ dương.

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> Dummy</u>($n$):<br/>
&nbsp;&nbsp;&nbsp;  <b>if </b>$n = 1$ or $n = 2$<br/>
&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;  return $1$<br/>
&nbsp;&nbsp;&nbsp; return <span style="font: normal verdana; font-variant: small-caps">Dummy</span>($n-1$) + <span style="font: normal verdana; font-variant: small-caps"> Dummy</span>($n-2$) + <span style="font: normal verdana; font-variant: small-caps"> Dummy</span>($n-3$)<br/>
</div>
<br/>

# 2. Phân tích tài nguyên tính toán




Khi phân tích một thuật toán, ta thường quan tâm đến <b>bộ nhớ</b> và <b>thời gian tính toán</b>. Khi chúng ta đã thành thạo phân tích một trong hai tài nguyên thì phân tích tài nguyên còn lại không phải quá khó khăn. Do đó, ở đây ta sẽ chỉ tập trung nói về phân tích thời gian tính toán của một thuật toán.

Phân tích một thuật toán về cơ bản là đếm <b>số thao tác cơ bản</b> mà thuật toán đó thực hiện. Định nghĩa chính xác thế nào là một thao tác cơ bản là việc <a href="http://www.giaithuatlaptrinh.com/?p=1719" rel="noopener" target="_blank">không tầm thường</a>. Tuy nhiên, để đơn giản, ta tạm coi các thao tác cơ bản ở đây là: cộng, trừ, nhân, chia, so sánh và mỗi thao tác cơ bản này mất 1 đơn vị thời gian. Do đó, đôi khi ta cũng coi số thao tác cơ bản như là một **ước lượng thô** của thời gian tính toán. Ta nói ước lượng thô là vì thời gian thực phụ thuộc rất nhiều vào máy tính (hay mô hình tính toán) mà ta sử dụng. Cùng 1 triệu phép nhân số 32 bít nhưng thời gian tính toán của phần cứng khác nhau có thể khác nhau. Tuy nhiên, ta vẫn chấp nhận cách ước lượng thô này vì nó sẽ làm phép phân tích thuật toán đơn giản hơn, loại bỏ sự phụ thuộc phần cứng ra khỏi phép phân tích.

Trong các phép phân tích, ta thường hay sử dụng [big-O](https://giaithuatlaptrinh.github.io/Khái-niệm-tiệm-cận/)  để phân tích thuật toán. 

## 2.1 Tại sao dùng Big-O?

Cách giải thích tốt nhất có lẽ là minh họa thông qua ví dụ.

> **Ví dụ 4:** phân tích thời gian của thuật toán <span style="font: normal verdana; font-variant: small-caps"> SumOfArray</span>($A[1,2,\ldots, n]$) trong ví dụ 1.

Thuật toán <font style="font: normal verdana; font-variant: small-caps"> SumOfArray</font> nhận đầu vào là một mảng $A[1,\ldots, n]$ với $n$ phần tử và trả lại tổng $A[1] + A[2] + \ldots + A[n]$.

Ta sẽ đếm số phép cộng của thuật toán trước. Mỗi lần lặp, thuật toán <font style="font: normal verdana; font-variant: small-caps"> SumOfArray</font> thực hiện một phép cộng để cập nhật $s$. Do ta có $n$ vòng lặp, thuật toán thực hiện $n$ phép cộng để tìm $s$.

Liệu thuật toán <font style="font: normal verdana; font-variant: small-caps"> SumOfArray</font> chỉ thực hiện $n$ phép cộng? Không hẳn vậy. Nếu bạn lập trình bằng ngôn ngữ C chẳng hạn, thì mỗi lần thực hiện vòng lặp for, bạn phải tăng biến đếm $i$ lên $1$. Do đó, ta phải tính cả số phép cộng thực hiện trên biến $i$ mà không phải chỉ với biến $s$. Số phép cộng tăng lên $2n$.

Nếu coi mỗi phép cộng mất 1 đơn vị thời gian thì thời gian của thuật toán <font style="font: normal verdana; font-variant: small-caps"> SumOfArray</font>  có phải là $2n$ đơn vị thời gian? Cũng không hẳn như vậy. Sau mỗi lần lặp, thuật toán còn phải so sánh $i$ với $n$ để  kiểm tra điều kiện kết thúc vòng lặp. Do đó, thuật toán sử dụng thêm $n$ phép so sánh. Vẫn chưa hết, thuật toán còn sử dụng $n$ phép gán với biến $s$ và $n$ phép gán với biến $i$. Tóm lại, thuật toán sử dụng khoảng $5n$ phép toán cơ bản nếu bạn thực thi bằng $C$.

Nếu bạn thực thi đoạn code trên bằng ngôn ngữ khác $C$, số lượng phép toán cơ bản có thể nhiều hơn như vậy. Tưởng tượng với đoạn code rất đơn giản trên mà thực hiện đếm chính xác số lượng phép toán cơ bản đã không tầm thường thì với các đoạn chương trình phức tạp hơn thì ta làm thế nào? Big-O $O(.)$ sẽ giúp ta đơn giản hoá bài toán hơn!

Thay vì đếm chính xác, $O(.)$ cho phép ta đếm <b>tương đối</b> số thao tác cơ bản. Theo phân tích ở trên, dù ta thực thi thuật toán <font style="font: normal verdana; font-variant: small-caps"> SumOfArray</font> bằng bất kì ngôn ngữ nào thì số lượng phép toán cơ bản đều không quá $C\cdot n$ với một hằng số $C$ nào đó. Hằng số $C$ có thể là 5 hoặc 10 hoặc 4. Ta gọi nó là hằng số là vì giá trị của nó không phụ thuộc vào $n$. Theo [định nghĩa hình thức của big-O](https://giaithuatlaptrinh.github.io/Khái-niệm-tiệm-cận/), ta có thể nói:
<p style="margin-left:2em;">Thuật toán <font style="font: normal verdana; font-variant: small-caps"> SumOfArray</font> có thời gian $O(n)$.</p>
Tóm lại, khi ta nói một thuật toán có độ phức tạp $O(f(n))$, ta muốn nói số lượng phép toán cơ bản mà thuật toán sử dụng không quá $C \cdot f(n)$ với một hằng số $C$ nào đó khi $n$ <b>đủ lớn</b>, bất chấp ngôn ngữ lập trình sử dụng. Như vậy, khi dùng $O(.)$ để phân tích thuật toán, ta có thể loại bỏ sự phụ thuộc vào  <b>ngôn ngữ lập trình</b> (hay thực thi mức thấp) của thuật toán.

Xét thêm ví dụ phức tạp hơn.

> **Ví dụ 5:** Phân tích thuật toán.
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> BubbleSort</u>($A[1,2,\ldots, n]$):<br/>
&nbsp;&nbsp;&nbsp; <b>for</b> $i \leftarrow 1$ to $n-1$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <b>for</b> $j \leftarrow i+1$ to $n$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <b>if</b> $A[i] $ &gt; $A[j]$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $tmp \leftarrow A[i]$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $A[i] \leftarrow A[j]$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $A[j] \leftarrow tmp$<br/>
&nbsp;&nbsp;&nbsp; return $A[1,2,\ldots, n]$<br/>
</div>
<br/>
Có lẽ bạn đọc cũng nhận ra thuật toán <font style="font: normal verdana; font-variant: small-caps"> BubbleSort</font> thực hiện sắp xếp một mảng đầu vào theo chiều tăng dần. (Việc hiểu thuật toán thực sự thực hiện tác vụ gì đôi khi không cần thiết để phân tích thuật toán.) Để phân tích thuật toán này, ta chỉ cần đếm số phép so sánh các phần tử của mảng $A[1,\ldots, n] $ (số lần kiểm tra điều kiện của câu lệnh **if**). Tại sao? Ngoại trừ khi $n = 1$, mỗi khi ta thực hiện thao tác bất kì (gán, đổi chỗ, v.v), ta đều thực hiện một phép so sánh. Do đó, chỉ cần đếm số phép so sánh và sử dụng khái niệm $O(.)$ là ta coi như đã xong.

Do mỗi lần lặp ta sử dụng một phép so sánh, thay vì đếm số phép so sánh, ta đếm số lần lặp. Với mỗi $i$ cố định, vòng lặp trong cùng có $n-(i+1) + 1 = n-i$ lần lặp. Do đó, tổng số lần lặp là:
<p style="text-align: center;"> $\sum_{i=1}^{n-1} n-i =  \sum_{j=1}^{n-1} j = n (n-1)/2 = n^2/2 - n/2 \qquad (1)$</p>
Như vậy, độ phức tạp của thuật toán <font style="font: normal verdana; font-variant: small-caps"> BubbleSort</font> là $O(n^2/2-n/2)$.

Theo định nghĩa của $O(.)$, ta có thể loại bỏ hằng nhân tử hằng số trong biểu thức. Do đó, ta có thể viết   $O(n^2/2-n/2) = O(n^2-n) = O(n^2)$. Tổng kết lại, ta có:
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><b>Định lý 1: </b>Độ phức tạp của thuật toán <font style="font: normal verdana; font-variant: small-caps"> BubbleSort</font> là $O(n^2)$.</div>
&nbsp;

Từ phân tích ví dụ 2, ta có thể thấy rằng kí hiệu $O(.)$  cho phép chúng ta biểu diễn thời gian tính toán bằng các biểu thức đơn giản hơn. Không chỉ có vậy, $O(.)$  còn cho phép chúng ta phát triển các công cụ phân tích mạnh hơn, tổng quát hơn nhưng lại dễ hiểu hơn. Để minh họa điểm này, ta xét ví dụ sau.

> **Ví dụ 6:** Phân tích thuật toán sau
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> MergeSort</u>($A[1,2,\ldots, n]$):<br/>
&nbsp;&nbsp;&nbsp; <b>if</b> $n$ &gt; $1$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; $m \leftarrow \lfloor n/2\rfloor$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;<font style="font: normal verdana; font-variant: small-caps"> MergeSort</font>($A[1,2,\ldots, m]$)<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;<font style="font: normal verdana; font-variant: small-caps"> MergeSort</font>($A[m+1,2,\ldots, n]$)<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;<font style="font: normal verdana; font-variant: small-caps"> Merge</font>($A[1,2,\ldots, n],m$)<br/>
</div>
<br/>
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> Merge</u>($A[1,2,\ldots, n]$):<br/>
&nbsp;&nbsp;&nbsp; $i \leftarrow 1; j \leftarrow m+1$<br/>
&nbsp;&nbsp;&nbsp; <b>for </b>$k \leftarrow 1$ to $n$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <b>if</b> $j$ &gt; $ n$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $B[k] \leftarrow A[i]; i \leftarrow i + 1$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <b>else if</b> $i$ &gt; $m$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $B[k] \leftarrow A[j]; j \leftarrow j + 1$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <b>else if</b> $A[i]$ &gt; $A[j] $<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $B[k] \leftarrow A[i]; i \leftarrow i + 1$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <b>else</b><br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $B[k] \leftarrow A[j]; j \leftarrow j + 1$<br/>
&nbsp;&nbsp;&nbsp; <b>for </b>$k \leftarrow 1$ to $n$<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $A[k] \leftarrow B[k]$<br/>
</div>
<br/>
Để phân tích thuật toán trên trước hết bạn hãy tự thuyết phục mình rằng thuật toán trên sẽ dừng sử dụng quy nạp. ( Bạn không nhất thiết phải hiểu thuật toán trên thực hiện tác vụ gì; nếu bạn thực sự muốn biết thì thuật toán trên thực hiện sắp xếp một mảng theo chiều tăng dần.) 

Ta sẽ dùng phương pháp giải hệ thức truy hồi để phân tích thuật toán  . Ta gọi $T(n)$ là độ phức tạp của thuật toán <font style="font: normal verdana; font-variant: small-caps"> MergeSort</font> khi mảng đầu vào có $n$ phần tử. Độ phức tạp của hai thủ tục gọi đệ quy lần lượt là $T(\lfloor n/2 \rfloor)$ và $T(\lceil n/2 \rceil)$. Phân còn lại của phép phân tích là tính độ phức tạp của thủ tục <font style="font: normal verdana; font-variant: small-caps"> Merge</font>.

Nếu bạn đếm chính xác số thao tác cơ bản của thủ tục <font style="font: normal verdana; font-variant: small-caps"> Merge</font> là một điều không hề dễ dàng vì có các lệnh điều kiện <b>if-else</b>. Tuy nhiên, sử dụng $O(.)$, ta có thể khẳng định số thao tác cơ bản là $O(n)$ vì ta chỉ lặp $n$ lần và mỗi lần lặp ta thực hiện $O(1)$ phép tính cơ bản. Do đó, ta có:
<p style="text-align: center;"> $T(n) = T(\lceil n/2 \rceil) + T(\lfloor n/2 \rfloor) + O(n) \qquad (2)$</p>

Phương trình $(2)$ trông khá đẹp, nhưng giải nó không hề dễ vì có các hàm $\lceil \cdot \rceil$ và $\lfloor \cdot \rfloor$. Tuy nhiên, $O(.)$ cho phép chúng ta bỏ $\lceil \cdot \rceil$ và $\lceil \cdot \rceil$ ra khỏi phương trình (chứng minh tính chất này một cách tỉ mỉ không phải đơn giản, nhưng vẫn có thể làm được). Ta được:
<p style="text-align: center;"> $T(n) = 2T(n/2)  + O(n) \qquad (3)$</p>

Giải phương trình $(3)$, ta chỉ cần áp dụng [định lí thợ](https://giaithuatlaptrinh.github.io/Hệ-thức-truy-hồi/) mà ta đã học ở bài trước (xem ví dụ 3 trong bài đó), thu được $T(n) = O(n\log n)$.

Rõ ràng thuật toán <font style="font: normal verdana; font-variant: small-caps"> MergeSort</font>  trông khá phức tạp nhưng $O(.)$ cho phép chúng ta phân tích nó một cách khá đơn giản.

## 2.2 Phân tích sử dụng $o(.), \Theta(.)$ và $\Omega(.)$

Theo phân tích ở trên, thuật toán <font style="font: normal verdana; font-variant: small-caps"> BubbleSort</font>  có độ phức tạp $O(n^2)$. Theo định nghĩa của $O(.)$, sẽ không sai nếu ta nói <font style="font: normal verdana; font-variant: small-caps"> BubbleSort</font> có độ phức tạp $O(n^3)$ do $n^2 = O(n^3)$. Tuy nhiên, nói như vậy chưa thể hiện đúng nhất độ phức tạp của thuật toán <font style="font: normal verdana; font-variant: small-caps"> BubbleSort</font>. Thông thường, khi ta nói độ phức tạp của thuật toán là $O(T(n))$ thì ta cố gắng tìm biểu thức $T(n)$ gần nhất với độ phức tạp thực sự của thuật toán mà ta có thể chứng minh được. Các kí hiệu $o(.), \Theta(.), \Omega(.)$ sẽ giúp chúng ta biểu diễn về mặt lý thuyết biểu thức $O(.)$ có phản ánh đúng hiệu năng của thuật toán hay không. Để minh họa, ta xét thêm một ví dụ nữa.

> **Ví dụ 7:** Tìm kiếm nhị phân.
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> BinarySearch</u>($A[1,2,\ldots,n],a$):<br/>
&nbsp;&nbsp;&nbsp; <b>if</b> $n=1$<br/>
&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp; return $n$<br/>
&nbsp;&nbsp;&nbsp; <b>else</b><br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $m \leftarrow \lfloor n/2 \rfloor$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <b>if</b> $a$ &lt; $A[m]$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font>($A[1,2,\ldots,m-1],a$)<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <b>else if</b> $a$ &gt; $A[m]$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font>($A[m+1,2,\ldots,n],a$)<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <b>else </b><br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return $m$<br/>
</div>
<br/>
Thuật toán <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font> tìm kiếm vị trí của phần tử có giá trị $a$ trong mảng $A[1,\ldots, n]$ đã sắp xếp theo chiều tăng dần. Trong giả mã trên, ta giả sử $a$ luôn xuất hiện trong $A[1,\ldots, n]$.

Không khó để thấy thuật toán trên có độ phức tạp $O(\log n)$. Tuy nhiên, không phải lúc nào thuật toán cũng cần tới $O(\log n)$ thao tác. Ví dụ khi $A[1,\ldots, 2n] = [1,2,\ldots, 2n]$ và $ a = n$ thì chỉ cần $O(1)$ thao tác thôi ta đã tìm được vị trí của phần tử $a$ rồi. Câu hỏi đặt ra là liệu cận trên  $O(\log n)$ đã chặt chưa? Sử dụng kí hiệu $o(.)$, ta có thể phát biểu câu hỏi tương đương là liệu thời gian của thuật toán <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font> có phải là $o(\log n)$?

Câu trả lời là không. <b>Với mọi</b> $n \geq 1$ và mọi mảng $A[1,2,\ldots, n]$ đã sắp xếp, luôn <b>tồn tại</b> $a$ sao cho thuật toán <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font>  cần <b>ít nhất</b>  $\lfloor \log n \rfloor$ phép so sánh để có thể xác định được vị trí của $a$ trong mảng  $A[1,2,\ldots, n]$. Ta nói thuật toán <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font> có độ phức tạp $\Omega(\log n)$. Không khó để chứng minh rằng $a = A[n]$ sẽ thỏa mãn phát biểu trên.

Như vậy, cận trên độ phức tạp của thuật toán <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font> là $O(\log n)$ và cận dưới  độ phức tạp của thuật toán <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font> là $\Omega(\log n)$, ta nói thuật toán <font style="font: normal verdana; font-variant: small-caps"> BinarySearch</font> có độ phức tạp $\Theta(\log n)$. Kí hiệu  $\Theta(.)$ dùng khi biểu thức trong cận trên $O(.)$ và cận dưới $\Omega(.)$ trùng với nhau.
