# Bài 11: Cây khung nhỏ nhất

[comment]: <> 
Trong bài này, chúng ta tìm hiểu bài toán tìm cây khung nhỏ nhất (minimum spanning tree) của một đồ thị đơn, vô hướng và liên thông. Đây là một bài toán điển hình trên đồ thị, có rất nhiều ứng dụng thực tiễn trong thiết kế mạng.

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <span style="color:dodgerblue">Bài toán cây khung nhỏ nhất: </span> Cho đồ thị vô hướng, liên thông $G(V,E)$ trong đó mỗi cạnh $e \in E$ được gán một trọng số $w(e) \in \mathbb{R}^+$. Một <b>cây khung</b> là một đồ thị con của $G$, không có chu trình, và chứa mọi đỉnh của $G$. Trọng số của một cây khung là tổng trọng số các cạnh của cây khung đó. Tìm cây khung có trọng số nhỏ nhất của $G(V,E)$.</div>
<br/>

> Ví dụ 1: Đồ thị (hình (1)) với trọng số và một cây khung nhỏ nhất (hình (2)) với trọng số 13.<br/>
> <img src="http://www.giaithuatlaptrinh.com/wp-content/uploads/2016/05/mst-ex.png" alt="mst-ex">

Ta sẽ tìm hiểu một số tính chất cơ bản của cây khung mà ta có thể sử dụng trong thuật toán tìm cây khung nhỏ nhất.

# 1. Một số tính chất của cây khung

Một số tính chất chung của một cây khung (không nhất thiết là nhỏ nhất):

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> Gọi $T$ là một cây khung của đồ thị (liên thông) $G(V,E)$ với $n$ đỉnh và $m$ cạnh.
<ol>
<li>$T$ có đúng $n-1$ cạnh.</li>
<li>Với mỗi cặp đỉnh $u,v$, tồn tại một và chỉ một đường đi nối hai đỉnh $u,v$ trong $T$.</li>
<li>Thêm bất kì một cạnh nào vào $T$ sẽ tạo ra một (và chỉ một) chu trình.</li>
</ol>
</div><br/>

Chứng minh các tính chất trên không khó, và ta sẽ coi như bài tập cho bạn đọc. 

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><span style="color:dodgerblue"><b> Lát cắt (Cut): </b></span> Gọi $A$ là một tập đỉnh của đồ thị. Lát cắt tương ứng với $A$, kí hiệu là $(A,V\setminus A)$, là tập <b>tất cả</b> các cạnh của $G$ có đúng một đầu mút  nằm trong $A$ (đầu mút còn lại nằm trong $V\setminus A$).</div><br/>

> Ví dụ 2: Lát cắt tương ứng với $\{a,b,c\}$ của đồ thị trong hình (a) là các cạnh màu đỏ trong hình (b). <br/>
><img src="http://www.giaithuatlaptrinh.com/wp-content/uploads/2016/05/cut-example.png" alt="cut-example">

Một số tính chất sau của cây khung  <b>nhỏ nhất</b> sẽ rất hữu ích trong thiết kế thuật toán.
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;">
<ol>
<li value="4"><b>Tính Chu Trình (Cycle Property):</b> Cạnh có trọng số lớn hơn các cạnh khác trong cùng một chu trình (nào đó) của đồ thị <b>không</b> nằm trong bất kì cây khung nhỏ nhất nào.</li>
<li><b>Tính Cắt (Cut Property):</b> Cạnh có trọng số nhỏ hơn các cạnh khác trong cùng một lát cắt (nào đó) của đồ thị $G$ nằm trong <b>mọi</b> cây khung nhỏ nhất.</li>
<li>Nếu có duy nhất một cạnh có trọng số nhỏ nhất thì bất kì cây khung nhỏ nhất nào cũng phải chứa cạnh này.</li>
</ol>
</div><br/>

Phương pháp chứng minh các tính chất trên đều giống nhau, sử dụng phương pháp lập luận tráo đổi (exchange argument). Ta sẽ chứng minh tính chất (4) và (5) để minh họa cho phương pháp chứng minh này.  Chứng minh (6) dễ hơn (4) và (5); chi tiết coi như bài tập cho bạn đọc.

**Chứng minh:** (4) Giả sử cạnh $e$ lớn hơn các cạnh khác trong cùng một chu trình $C$ và giả sử một cây khung nhỏ nhất $T$ chứa $e$. Xóa $e$ khỏi $T$, ta sẽ thu được hai cây $F_1,F_2$. Do $C$ là một chu trình, tồn tại một cạnh khác $e$, gọi là $f$,  của $T$ nối $F_1$ với $F_2$. Do đó $T\cup \{f\} - \{e\}$ là một cây khung của $G$ có trọng số nhỏ hơn $T$ (vì $w(f) < w(e)$), trái với giả thiết $T$ là một cây khung nhỏ  nhất.

(5) Giả sử cạnh $e$ nhỏ hơn các cạnh khác trong cùng một lát cắt $L$ và giả sử tồn tại một cây khung nhỏ nhất $T$ <b>không</b> chứa $e$. Thêm $e$ vào $T$, ta sẽ thu được một chu trình $C$ chứa $e$. Do $L$ là lát cắt, $C$ phải chứa ít nhất một cạnh $f\not= e$ của $L$. Do đó, $T\cup \{e\} - \{f\}$ là một cây khung của $G$ có trọng số nhỏ hơn $T$ (do $w(e) > w(f)$), trái với giả thiết $T$ là một cây khung nhỏ  nhất. $\blacksquare$


# 2. Thuật toán Kruskal

Thuật toán Kruskal là một thuật toán kiểu [tham lam](https://giaithuatlaptrinh.github.io/Giải-thuật-tham-lam/), cực kì đơn giản và có thể mô tả bằn một câu:

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><b> Kruskal: </b>Tại mỗi bước, đưa cạnh <i>nhỏ nhất</i> trong số các cạnh còn lại vào cây khung, <i>nếu có thể</i>.
</div>
<br/>
Ta sẽ cắt nghĩa từ *nếu có thể* ở trên. Tưởng tượng ta sẽ duy trì một tập cạnh $T \subseteq E$, ban đầu khởi tạo rỗng. Khi thuật toán kết thúc, $T$ sẽ là một cây khung (nhỏ nhất) của $G$. Tại mỗi bước, ta sẽ xét một cạnh $e$, nếu $T\cup e$ không có chu trình thì $e$ sẽ được gọi là cạnh *có thể* thêm vào $T$.  Theo mô tả ở  , các cạnh ta xét sẽ theo thứ tự tăng dần của trọng số, vì ta muốn đưa nhiều cạnh có trọng số nhỏ vào $T$ và hi vọng các cạnh này tạo thành một cây khung nhỏ nhất. Giả mã của thuật toán Kruskal như sau:

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> Kruskal</u>($G(V,E),w$):<br/>
1.&nbsp;&nbsp;&nbsp; $T \leftarrow (V, \emptyset)$    &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;  $\ll $ there is no edge in $T \gg$<br/>
2.&nbsp;&nbsp;&nbsp; sort edges in $E$ in $\uparrow$ order of weight<br/>
3.&nbsp;&nbsp;&nbsp; let $e_1,\ldots, e_m$ be edges in the sorted ofder.<br/>
4.&nbsp;&nbsp;&nbsp; <b>for</b> $i \leftarrow 1$ to $m$<br/>
5.&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <font color="red"><b>if</b> $T\cup \{e_i\}$ has no cycle</font><br/>
6.&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $T\leftarrow T\cup \{e_i\}$<br/>
7.&nbsp;&nbsp;&nbsp; return $T$.<br/>
</div><br/>

> **Ví dụ 3**: Hình sau minh hoạ các bước chạy của thuật toán Kruskal với đồ thị trong ví dụ 1. Các cạnh màu đỏ là các cạnh nằm trong cây khung nhỏ nhất. Các cạnh màu xanh là các cạnh đã được xét và không nằm trong cây khung. Các cạnh màu xanh gạch ngang là cạnh sẽ được xem xét đưa vào cây khung trong bước tiếp theo. Thứ tự các cạnh được xét là theo chiều tăng dần của trọng số. <br/>
> <img src="http://www.giaithuatlaptrinh.com/wp-content/uploads/2016/05/kruskal-run.png" alt="kruskal-running-example">


Tất cả các bước của thuật toán Kruskal đều không khó thực thi, ngoại trừ bước số 6. Để kiểm tra một đồ thị (vô hướng, có thể không liên thông) có chu trình hay không, ta chỉ cần thực hiện các thuật toán [duyệt đồ thị](https://giaithuatlaptrinh.github.io/Nhập-môn-đồ-thị/) (BFS và DFS đều được). Trong trường hợp này, phép duyệt có thể thực hiện trong thời gian $O(n)$, chứ không phải $O(n+m)$, và do đó thuật toán Kruskal có thể thực hiện được trong thời gian $O(mn)$. Tạm gác vấn đề thực thi qua một bện; ta sẽ tìm hiểu sâu hơn ở dưới và trong phần bài tập. Ta tập trung vào câu hỏi: tại sao đầu ra của thuật toán Kruskal là một cây khung nhỏ nhất?


## 2.1. Tính đúng đắn của thuật toán Kruskal

Ta phải chứng minh hai điều: (a) đầu ra của thuật toán là một cây khung và (b) cây  đó có trọng số nhỏ nhất trong số tất cả các cây khung của đồ thị. 

Ta sẽ chứng minh (a) trước. Đầu tiên, ta dễ thấy đầu ra $T$ không có chu trình vì dòng 5 của thuật toán sẽ đảm bảo điều này. Như vậy, $T$ sẽ là cây nếu liên thông (chỉ có duy nhất một thành phần liên thông). Giả sử đầu ra không liên thông, $T$ sẽ có ít nhât hai thành phần liên thông (mỗi thành phần liên thông có thể chỉ là 1 đỉnh). Chọn hai thành phần liên thông $F_1,F_2$ của $T$, sao cho tồn tại một cạnh của $G$ nối hai thành phần này. Ta luôn chọn được  vì $G$ liên thông. Gọi $e_k$ là cạnh có trọng số nhỏ nhất nối $F_1,F_2$. Cạnh $e_k$ này không thuộc $T$ vì nếu không $F_1,F_2$ sẽ là chỉ là một thành phần mà thôi. Nhưng khi kiểm tra điều kiện ở dòng màu đỏ, $T\cup \{e_k\}$ rõ ràng không có chu trình. Điều đó có nghĩa ta sẽ thêm $e_k$ vào trong $T$ ngay sau đó. Điều này trái với giả sử $e_k$ không thuộc $T$. Do đó $T$ phải liên thông, i.e, $T$ là một cây.

Giờ ta sẽ chứng minh (b). Do thuật toán Krukal là một thuật toán tham lam, ta sẽ dùng phép biện luận tráo đổi như đã áp dụng trong bài thuật toán [tham lam](https://giaithuatlaptrinh.github.io/Giải-thuật-tham-lam/).  Gọi $F$ là một cây khung nhỏ nhất của $G$ sao cho số cạnh chung giữa $F$ và $T$ là lớn nhất. Nếu $F = T$ thì rõ ràng $T$ là một cây khung nhỏ nhất. Do đó, giả sử $F \not= T$.

Gọi $T = \{t_1,t_2, \ldots, t_{n-1}\}$ và $F = \{f_1,f_2,\ldots,f_{n-1}\}$ lần lượt là thứ tự các cạnh của $T$ và $F$ sắp xếp theo chiều tăng của trọng số. Gọi $i$ là chỉ số nhỏ nhất sao cho cạnh $t_i$ và cạnh $f_i$ là hai cạnh khác nhau. Theo cách chọn $i$, ta suy ra $i \leq n-2$ và $f_k = t_k, 1 \leq  k \leq i-1$. Do đó, $\{t_1,\ldots, t_{i-1}\} \cup \{f_i\}$ không có chu trình.

Ta sẽ có $w(t_i) \leq w(f_i)$, vì nếu không, dòng màu đỏ trong thuật toán sẽ kiểm tra cạnh $f_i$ <b>trước khi</b> kiểm tra cạnh $t_i$, và do $\{t_1,\ldots, t_{i-1}\} \cup \{f_i\}$ không có chu trình, $f_i$ sẽ được thêm vào $T$ trước $t_i$. Hay nói cách khác, $f_i$ chính là cạnh thứ $i$ của $T$; trái với giả thiết $f_i \not= t_i$.

Xét đồ thị $H = F \cup \{t_i\}$. $H$ sẽ có một chu trình $C$. Chu trình $C$ có hai tính chất sau:
<ol>
<li> $C$ chứa $t_i$. Nếu không, $C$ sẽ là một chu trình của $F$, trái với giả thiết $F$ là một cây.</li>
<li> $C$ chứa ít nhất một cạnh $f_j$ sao cho $j \geq i $. Nếu không, $C$ là tập con của $\{f_1,\ldots,f_{i-1}\} \cup {t_i}$; tập này cũng chính là tập $\{t_1,\ldots, t_i\}$. Hay nói cách khác $C$ là tập con của $T$, trái với tính chất $T$ là một cây.</li>
</ol>
Gọi $F' = H - \{f_j\}$; $F'$ là một cây (tại sao?). Do $w(f_j) \geq w(f_i) \geq w(t_i)$, $w(F') \leq w(F)$. Từ đó suy ra $F'$ cũng là một cây khung nhỏ nhất. Nhưng rõ ràng $F'$ có số cạnh chung với $T$ nhiều hơn $F$. Điều này trái với giả thiết $F$ là cây khung nhỏ nhất có số cạnh chung nhiều nhất với $T$. Như vậy, $F = T$, i.e, $T$ là cây khung nhỏ nhất.


## 2.2. Thực thi thuật toán Kruskal

Như đã chỉ ra ở trên, ta chỉ cần thực hiện dòng 5 của thuật toán Kruskal một cách hiệu quả. Phép duyệt đồ thị cho ta thuật toán với thời gian $O(n)$ để thực hiện dòng này. Phần này ta đưa ra một cách nhìn khác, cho phép ta thực thi nó hiệu quả hơn. 

Ta nhận xét thấy trong mỗi bước trung gian, $T$ là một rừng, i.e, mỗi thành phần liên thông của $T$ là một cây. Phép kiểm tra xem $T\cup \{e\}$ có chu trình hay không tương đương với kiểm tra xem hai đầu mút, gọi là $u,v$, của $e$ có thuộc **cùng một cây** trong rừng $T$ hay không.
<ol>
<li type="a">Nếu $u,v$ thuộc cùng một cây, $T\cup \{e\}$ sẽ có chu trình.</li>
<li>Ngược lại, $T\cup \{e\}$ không có chu trình, và phép thêm $e$ vào $T$ sẽ tương đương với gộp 2 cây con của $T$ thành một cây mới bằng cạnh $e$.</li>
</ol>
Ta có thể biểu diễn mỗi cây của $T$ bằng **một tập hợp**: các đỉnh thuộc cùng một cây khi và chỉ khí nó nằm trong cùng một tập. Giả sử mỗi tập có một ID duy nhất để định danh tập hợp đó; hai tập khác nhau có ID  khác nhau. Hai thao tác ta cần để thực hiện thuật toán Kruskal là:

<ol>
<li><b><font style="font: normal verdana; font-variant: small-caps">Find</font>($x$)</b>: Trả lại id của tập hợp chứa $x$.</li>
<li><b><font style="font: normal verdana; font-variant: small-caps">Union</font>($x,y$)</b>: Thay thế hai tập hợp $A,B$ lần lượt chứa $x$ và $y$ bằng tập hợp $A \cup B$. Ở đây ta giả sử phép <font style="font: normal verdana; font-variant: small-caps">Union</font> luôn gộp 2 phần tử thuộc hai tập khác nhau.</li>
</ol>

Ngoài ra, ta còn thêm thao tác <b><font style="font: normal verdana; font-variant: small-caps">MakeSet</font>($x$)</b> để tạo tập hợp  chỉ chứa một phần tử $\{x\}$. Khi đó thuật toán Kruskal có thể được thực hiện như sau: 

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> KruskalUnionFind</u>($G(V,E),w$):<br/>
1.&nbsp;&nbsp;&nbsp; $T \leftarrow (V, \emptyset)$    &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;  $\ll $ there is no edge in $T \gg$<br/>
2.&nbsp;&nbsp;&nbsp; <b>for each</b> $v \in V$<br/>
3.&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <font style="font: normal verdana; font-variant: small-caps"> MakeSet</font>($v$) <br/>
4.&nbsp;&nbsp;&nbsp; sort edges in $E$ in $\uparrow$ order of weight<br/>
5.&nbsp;&nbsp;&nbsp; let $e_1,\ldots, e_m$ be edges after sorting<br/>
6.&nbsp;&nbsp;&nbsp; <b>for</b> $i \leftarrow 1$ to $m$<br/>
7.&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; let $u,v$ be two endpoints of $e_i$<br/>
8.&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <font color="red"><b>if</b> <font style="font: normal verdana; font-variant: small-caps"> Find</font>$(u) \not= $ <font style="font: normal verdana; font-variant: small-caps"> Find</font>($v$)</font><br/>
9.&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $T\leftarrow T\cup \{e_i\}$<br/>
10.&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <font color="red"><font style="font: normal verdana; font-variant: small-caps"> Union</font>$(u,v)$</font><br/>
11.&nbsp;&nbsp;&nbsp; return $T$.<br/>
</div>
&nbsp;

**Nhắc lại cấu trúc tập hợp:** Trong bài viết này, mình giả sử bạn đọc đã làm quen với cấu trúc biểu diễn tập hợp sao cho thao tác trong dòng 8 và 10 thực hiện được trong thời gian $O(\log n)$ (thay vì $O(n)$ như phép duyệt đồ thị.) Ở đây mình chỉ nhắc lại cách thực thi các thao tác, chi tiết phân tích xem trong phần bài tập.

Ta sẽ dùng cấu trúc cây để biểu diễn một tập hợp. Mảng $P[1,\ldots,n]$ sẽ biểu diễn cấu trúc cây này. Nói cách khác, $P[v] = u$ nếu như $u$ là nút cha của $v$ trong cây (biểu diễn tập hợp chứa $u$ và $v$). Do đó, để kiểm tra xem hai phần tử $u,v$ có trong cùng một cây hay không, ta chỉ việc tìm xem nút gốc của cây biểu diễn có trùng nhau hay không. Gốc của cây cũng chính là ID của tập hợp.

Để phép tìm kiếm gốc của cây được thực hiện trong thời gian $O(\log n)$, ta  sẽ muốn biểu diễn tập hợp đó bằng một cây sao cho đường đi từ một nút bất kì đến gốc có độ dài không quá $O(\log n)$. Nói cách khác, ta muốn các cây này có độ sâu không quá $O(\log n)$. Ban đầu mỗi tập chỉ là $1$ phần tử (dòng 3). Kích thước tập hợp tăng lên do ta thực hiện phép hợp (Union) ở dòng 10. Khi hợp hai tập hợp $A$ và $B$ có gốc là $a,b$. Ta có hai lựa chọ: (1) cho $a$ làm cha của $b$ bằng cách thiết lập $P[b] \leftarrow a$ (và do đó nó sẽ là gốc của tập hợp mới $A\cup B$.), hoặc ngược lại, (2) cho $b$ làm cha của $a$ bằng cách thiết lập $P[a] \leftarrow b$. Nếu ta thực hiện một cách tuỳ tiện (1) hoặc (2), chiều sâu của cây có thể rất lớn, như chỉ ra trong ví dụ sau:

>  **Ví dụ 4:** Xét các tập $(v_1), (v_2), \ldots,(v_n)$ được gộp theo thứ tự: $(v_1)$ gộp với $(v_2)$ thành $(v_1,v_2)$; sau đó $(v_1,v_2)$ được gộp với $(v_3)$ thành $(v_1,v_2,v_3)$; $\ldots$; cuối cùng $(v_1,\ldots, v_{n-1})$ được gộp với $(v_n)$ để tạo thành $(v_1,v_2,\ldots, v_n)$. Nếu tại bước thứ $i$, ta để $v_{i+1}$ làm gốc của tập $(v_1,v_2,\ldots, v_{i+1})$ thì cây cuối cùng sẽ là một đường đi $v_1 \rightarrow v_2 \rightarrow \ldots \rightarrow v_n$ gốc tại $v_n$; nói cách khác, nó có độ sâu $n-1$. Đây không phải là điều ta muốn. 

Ví dụ 4 cho ta một ý tưởng chọn gốc: khi gộp tập $A,B$ tương ứng với gốc tại $a,b$, ta sẽ để nút biểu diễn tập **có kích thước lớn hơn**  làm gốc của tập mới; nếu hai tập có kích thước bằng nhau thì chọn tuỳ ý một trong hai nút. Áp dụng cho ví dụ 4, ta sẽ thấy cây thu được có độ sâu $1$; nhỏ hơn nhiều so với độ sâu mong muốn $O(\log n)$.

Để thực hiện gộp theo kích thước, ta sẽ lưu trữ ở gốc $u$ của mỗi cây một biến $S[u]$ lưu trữ kích thước của cây. Giả mã của các thao tác biểu diễn tập hợp của ta sẽ như sau:

Giả mã:
<div style=" width: 120px; padding: 6px; color: black; background-color: white; border: black 2px solid;"> <u style="font: normal verdana; font-variant: small-caps"> MakeSet</u>($x$):<br/>
&nbsp;&nbsp;&nbsp; $P[x] \leftarrow x$<br/>
&nbsp;&nbsp;&nbsp; $S[x] \leftarrow 1$<br/>
</div><br/>

<div style=" width: 300px; padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> Find</u>($x$):<br/>
&nbsp;&nbsp;&nbsp; <b>if</b> $P[x] \not= x$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $P[x] \leftarrow $ <font style="font: normal verdana; font-variant: small-caps"> Find</font>($P[x]$)<br/>
&nbsp;&nbsp;&nbsp; return $P[x]$<br/>
</div><br/>
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <u style="font: normal verdana; font-variant: small-caps"> Union</u>($x,y$):<br/>
&nbsp;&nbsp;&nbsp; $\bar{x} \leftarrow $ <font style="font: normal verdana; font-variant: small-caps"> Find</font>($x$)    &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; <i>$\ll$the root of $A\gg$</i><br/>
&nbsp;&nbsp;&nbsp; $\bar{y}\leftarrow $ <font style="font: normal verdana; font-variant: small-caps"> Find</font>($y$)    &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; <i>$\ll$the root of $B\gg $</i><br/>
&nbsp;&nbsp;&nbsp; <b>if</b> $S[\bar{x}]> S[\bar{y}]$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $P[\bar{y}] \leftarrow \bar{x}$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $S[\bar{x}] \leftarrow S[\bar{x}]  + S[\bar{y}] $<br/>
&nbsp;&nbsp;&nbsp; <b>else</b><br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $P[\bar{x}]  \leftarrow \bar{y}$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $S[\bar{y}] \leftarrow S[\bar{x}]  + S[\bar{y}] $<br/>
</div><br/>
Ta có tính chất sau; chứng minh xem trong phần bài tập.

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <span style="color:dodgerblue">Tính chất 1: </span> Độ sâu của cây biểu diễn tập hợp $A$ là $O(\log |A|)$ trong đó $|A|$ là số phần tử trong tập $A$.</div>
<br/>

**Phân tích thời gian thuật toán Kruskal:** Sắp xếp các cạnh của đồ thị theo trọng số mất thời gian $O(m\log m) = O(m \log n)$ (dùng  [merge sort](https://giaithuatlaptrinh.github.io/Chia-để-trị-I/) chẳng hạn). Theo tính chất 1, mỗi thao tác <span style="font: normal verdana; font-variant: small-caps"> Find</span>($x$) và <span style="font: normal verdana; font-variant: small-caps"> Union</span>($x,y$) có độ phức tạp không quá $O(\log n)$. Do đó, vòng lặp  trong dòng 6-11 có thể thực hiện trong thời gian  $m \log n$. Vậy tổng thời gian của thuật toán là $O(m\log n)$. $\blacksquare$


# 3. Thuật toán Prim


Thuật toán Prim [5] tìm cây khung nhỏ nhất dựa trên tính cắt (tính chất 5) của cây khung. Thuật toán duy trì một tập đỉnh $A$ và một cây khung $T$ cho tập đỉnh này. Ban đầu, $A$ chỉ có một đỉnh (bất kì) của đồ thị và $T$ không chứa cạnh nào. Mỗi bước, thuật toán tìm ra đỉnh $u$ không thuộc $A$ sao cho cạnh kề với $u$ là cạnh có trọng số nhỏ nhất trong số các cạnh trong lắt cắt $(A,V\setminus A)$ và thêm $u$ vào $A$. Trong giả mã dưới đây, ta sử dụng $T$ để biểu diễn cả tập $A$ và cây khung của tập $A$ này. Cụ thể, lát cắt $(T,V\setminus T)$ sẽ là lát cắt tương ứng với các đỉnh trong $T$.

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <u style="font: normal verdana; font-variant: small-caps"> Prim</u>($G(V,E),w$):<br/>
1.&nbsp;&nbsp;&nbsp; pick any vertex $u$<br/>
2.&nbsp;&nbsp;&nbsp; $T\leftarrow \{u \}$<br/>
3.&nbsp;&nbsp;&nbsp; <b>while</b> $T \not= V$     &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $\ll T$ does not contain all vertices$\gg$<br/>
4.<font color="red">   &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; $e \leftarrow$ the smallest edge of the cut $(T,V\setminus T)$</font><br/>
5.&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; $T \leftarrow T \cup \{e\}$<br/>
6.&nbsp;&nbsp;&nbsp; return $T$<br/>
</div><br/>

> **Ví dụ 5:** Hình dưới đây minh hoạ các bước thực hiện thuật toán Prim.  Các cạnh màu đỏ là các cạnh mà ta đã thêm vào cây khung nhỏ nhất. Các cạnh màu xanh gạch ngang là các cạnh của lát cắt mà ta xét để tìm ra cạnh cần thêm vào cây khung.<br/>
> <img src="http://www.giaithuatlaptrinh.com/wp-content/uploads/2016/05/prim-example.png" alt="prim-example">


## 3.1. Tính đúng đắn của thuật toán Prim

Không khó để thấy rằng đầu ra của thuật toán Prim là một cây, vì ta không bao giờ đưa vào $T$ một cạnh giữa hai đỉnh đã thuộc $T$, theo định nghĩa của lát cắt. Do đó, ta chỉ còn cần chứng minh đầu ra là một cây khung nhỏ nhất.

Nếu mọi cạnh của đồ thị đều đôi một khác nhau, ta luôn đưa vào $T$ cạnh nhỏ nhất của một lát cắt, mà cụ thể là lát cắt ở dòng 4. Do đó, theo tính chất 5, $T$ phải là một cây khung nhỏ nhất (và cũng là duy nhất).

Trường hợp tồn tại các cạnh có cùng trọng số, để chứng minh tính đúng đắn của thuật toán Prim, ta cần chứng minh bổ đề sau:

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><span style="color:dodgerblue"><b> Bổ đề 1: </b></span > Gọi $e$ là cạnh có trọng số nhỏ nhất của một lát cắt $(A,V\setminus A)$. Tồn tại một cây khung nhỏ nhất $T$ của đồ thị chứa cạnh $e$.</div>
<br/>
**Chứng minh:** Thật vậy, gọi $F$ là một cây khung nhỏ nhất nhất không chứa $e$. Ta có $F\cup \{e\}$ sẽ chứa một chu trình $C$. Chu trình này phải chứ $e$. Do đó, $|C \cap (A, V\setminus A)| \not= \emptyset$. Do $C$ là một chu trình, nó sẽ chứa một số chẵn cạnh của lát cắt $(A, V\setminus A)$; nói cách khác, tồn tại một cạnh $f$ khác $e$ nằm trong cắt $(A, V\setminus A)$. Thêm nữa, $w(f)\geq w(e)$, do $e$ là cạnh có trọng số nhỏ nhất.

<img src="http://www.giaithuatlaptrinh.com/wp-content/uploads/2016/05/cut-edge-mst.png" alt="cut-edge-in-mst">

Gọi $T = F\cup \{e\} \setminus \{f\}$. $T$ là một cây khung của đồ thị và do $w(e) \leq w(f)$, ta suy ra $w(T) \leq w(F)$. Do đó, $T$ cũng phải là một cây khung nhỏ nhất vì $F$ là cây khung nhỏ nhất. Như vậy, $T$ là một cây khung nhỏ nhất chứa $e$. $\blacksquare$

Theo bổ đề $1$, đầu ra của thuật toán Prim cũng phải là cây khung nhỏ nhất mỗi cạnh của nó là một cạnh có trọng số nhỏ nhất của một lát cắt nào đó. $\blacksquare$

## 3.2. Thực thi thuật toán Prim

 Gọi $n,m$ lần lượt là số đỉnh và số cạnh của đồ thị. Ta thấy thời gian thực thi thuật toán là $O(nF(n,m))$ trong đó $F(n,m)$ là thời gian để thực thi dòng màu đỏ vì sau mỗi vòng lặp, số đỉnh của $T$ tăng lên $1$. Cách thực thi dòng màu đỏ đơn giản nhất đó là duyệt qua tất cả các cạnh của lát cắt và tìm cạnh có trọng số nhỏ nhất. Thời gian của cách thực thi này có thể lên tới $O(m)$, và như vậy, tổng thời gian của thuật toán là $O(nm)$. Với đồ thị dầy ($m = O(n^2)$), cách này cho chúng ta thuật toán $O(n^3)$. 
 
 Ta sẽ tìm hiểu một cách thực thi hiệu quả hơn, có thời gian $O(n^2)$. Ta sẽ sử dụng biểu diễn là ma trận kề. Mỗi đỉnh $u$ **không thuộc** cây hiện tại $T$ ($u \in V\setminus T$), ta sẽ lưu một nhãn $d[u]$. Nhãn $d[u]$ là trọng số của cạnh nhỏ nhất trong số các cạnh từ $u$ tới các đỉnh trong $T$ hiện tại. Ta cũng lưu thêm một nhãn $M[u]$, trong đó $M[u] = v$ nếu $v\in T$ và $w(u,v) = d[u]$.

Để tìm cạnh nhỏ nhất của lát cắt (thực hiện dòng màu đỏ), ta sẽ duyệt qua tất cả các đỉnh trong $ V\setminus T$, tìm ra đỉnh $u$ có $d[u]$ nhỏ nhất. Khi đó, cạnh $(u,M[u])$ chính là cạnh nhỏ nhất của lát cắt $(T, V\setminus T)$. Do đó, ta có thể thực hiện dòng màu đỏ với $F(n,m) = O(n)$. Liệu chúng ta đã xong?

Còn một vấn đề nữa, đó là sau khi thêm một cạnh vào trong cây khung hiện tại $T$, ta phải cập nhật lại nhãn cho các đỉnh còn lại. Giả sử cạnh $(u,v)$, với $v = M[u]$, sẽ được thêm vào cây khung. Giả sử trước khi thêm cạnh này, $u \not\in T$. Để cập nhật nhãn, ta duyệt qua các đỉnh còn lại trong $V\setminus T$ và với mỗi đỉnh $x$, ta so sánh trọng số $w(u,x)$ với $d[x]$. Nếu $w(u,x)< d[x]$, ta sẽ cập nhật $d[x] = w(u,x)$ và $M[x] = u$. Nếu không ta sẽ giữ nguyên trọng số. Thao tác cập nhật này có thể thực hiện trong thời gian $O(n)$, do đó, tổng thời gian của thuật toán sẽ là $O(n^2)$.
<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"><u style="font: normal verdana; font-variant: small-caps"> FastPrim</u>($G(V,E),w$):<br/>
&nbsp;&nbsp;&nbsp; pick any vertex $u$<br/>
&nbsp;&nbsp;&nbsp; $M[1,\ldots, n] \leftarrow \{0,\ldots,0\}$<br/>
&nbsp;&nbsp;&nbsp; $d[1, \ldots,n] \leftarrow \{\infty, \ldots, \infty\}$<br/>
&nbsp;&nbsp;&nbsp; <b>for</b> each neighbor $x$ of $u$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $M[x] \leftarrow u$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $d[x] \leftarrow w(u,x)$<br/>
&nbsp;&nbsp;&nbsp; $T \leftarrow \emptyset$<br/>
&nbsp;&nbsp;&nbsp; <b>while</b> $T \not= V$     &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $\ll T$ does not contain all vertices$\gg$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; $u \leftarrow$ <font style="font: normal verdana; font-variant: small-caps"> FindSmallestVertex</font>$(M[1,\ldots,V])$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; <b>for</b> each neighbor $x$ of $u$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <b>if</b> $x \not\in T$ and $w(u,x)$ &lt; $d[x]$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $M[x] \leftarrow u$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $d[x] \leftarrow w(u,x)$<br/>
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; $T \leftarrow T \cup \{(u,M[u])\}$       &nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; $\ll $ add edge $(u,M[u])$ to $ T\gg$<br/>
&nbsp;&nbsp;&nbsp; return $T$<br/>
</div><br/>

<div style="padding: 6px; color: black; background-color: white; border: black 2px solid;"> <u style="font: normal verdana; font-variant: small-caps"> FindSmallestVertex</u>($d[1,\ldots,n]$):<br/>
&nbsp;&nbsp;&nbsp; $min \leftarrow +\infty$<br/>
&nbsp;&nbsp;&nbsp;  $u \leftarrow 0$<br/>
&nbsp;&nbsp;&nbsp; <b>for</b> each $v \in V$<br/>
&nbsp;&nbsp;&nbsp;    &nbsp;&nbsp;&nbsp; <b>if</b> $d[v]$ &lt; $min$ and $v \not\in T$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $min \leftarrow d[v]$<br/>
&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp; $u \leftarrow v$<br/>
&nbsp;&nbsp;&nbsp; return $u$<br/>
</div><br/>

Khi giả mã trên, ta có thể nhận thấy từ mảng $M$, ta có thể xây dựng lại được cây $T$. Với mỗi $x$ mà $M[x] \not= 0$, $(x,M[x])$ chính là cạnh của cây $T$. Ta sẽ dùng thêm một mảng $inT[1,\ldots,n]$ trong đó $inT[u] = $ <font style="font: normal verdana; font-variant: small-caps"> True</font> nếu $u$ ở trong cây hiện tại và $inT[u] = $ <font style="font: normal verdana; font-variant: small-caps"> False</font> nếu ngược lại. 

> **Ghi chú 1:** Thực thi thuật toán Prim ở phần này chỉ phù hợp với đồ thị dầy với $\Omega(n^2)$ cạnh. Ta có thể thực thi thuật toán Prim với thời gian $O(m\log n)$ sử dụng cấu trúc Heap, như chi tiết sẽ phức tạp hơn thuật toán Kruskal.

> **Ghi chú 2:** Với đồ thị dầy, ta nên sử dụng thuật toán $O(n^2)$ của Prim, vì thực thi đơn giản mà thời gian chạy lại tối ưu. Với đồ thị thưa, ta nên sử dụng thuật toán Kruskal. 

> **Ghi chú 3:** Thuật toán Prim, theo <a href="https://en.wikipedia.org/wiki/Prim's_algorithm" target="_blank">Wikipedia</a>, được tìm ra đầu tiên bởi Jarník [2] năm 1930, sau đó được Prim [5] độc lập phát triển năm 1957. 


# 4. Tham khảo

[1] Kruskal, Joseph B. <i>On the Shortest Spanning Subtree of a Graph and the Traveling Salesman Problem.</i> Proceedings of the American Mathematical society 7.1 (1956): 48-50.<br/>
[2] Prim, Robert Clay. <i>Shortest Connection Networks and some Generalizations.</i> Bell System Technical Journal 36.6 (1957): 1389-1401.<br/>
[3] Borůvka, Otakar (1926). <i>Příspěvek k řešení otázky ekonomické stavby elektrovodních sítí (Contribution to the Solution of a Problem of Economical Construction of Electrical Networks)</i>. Elektronický Obzor (in Czech) 15: 153–154.<br/>
[4] Nešetřil, Jaroslav, Eva Milková, and Helena Nešetřilová. <i>Otakar Borůvka on minimum spanning tree problem translation of both the 1926 papers, comments, history.</i> Discrete Mathematics 233.1 (2001): 3-36.
[5] Karger, David R.; Klein, Philip N.; Tarjan, Robert E. <i>A Randomized Linear-time Algorithm to Find Minimum Spanning Trees</i>. Journal of the ACM 42 (2): 321–328, 1995.<br/>
[6] Chazelle, Bernard. <i>A Minimum Spanning Tree Algorithm with Inverse-Ackermann Type Complexity.</i> Journal of the ACM (JACM) 47.6 (2000): 1028-1047.<br/>
[7]Jarník, V. <i>O jistém problému minimálním" [About a certain minimal problem]</i>, Práce Moravské Přírodovědecké Společnosti (in Czech) 6: 57–63, 1930.<br/>
[8] Uri Zwick. <a href="http://www.cs.tau.ac.il/~zwick/grad-algo-13/mst.pdf" target="_blank"><i>Notes on Minimum Spanning Tree</i></a>. Tel Aviv University, 2013.<br/>



# 5. Bài tập
