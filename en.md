
[Source](https://threads-iiith.quora.com/Tutorial-on-Trie-and-example-problems "Permalink to Tutorial on Trie and example problems - Threads @ IIIT Hyderabad")

# Hướng dẫn và ví dụ về các vấn đề của Trie - Chủ đề của IIIT Hyderabad

Trong bài post này tôi sẽ viết về Tries và các khái niệm được sử dụng rộng rãi trong các vấn đề về thao tác. Chúng ra sẽ xem qua 2-3 vấn đề mà ở đó Trie khá hữu dụng.

Đầu tiên chúng ta tìm hiểu trie là gì. Trie có thể lưu trữ thông tin về keys/number/string một cách ngắn gọn trên 1 cấu trúc dạng cây. Trie chứa các mode mà ở đó mỗi node lại lưu trữ một kí tự/ hoặc một bit. Chúng ta có thể chèn thêm các string/number một cách phù hợp. 

![][1]


Nguồn: wikimpedia.

Nhưng chúng ta sẽ xử lý các con số ở đây, và đặc biệt là trên những bit nhị phân. Chúng ta sẽ tìm hiểu chúng khi chúng ta giải quyết các vấn đề.

**Vấn đề 1**: Cho một mảng các số nguyên, chúng ra phải tìm ra 2 phần tử mà XOR của nó tối đã.
**Giải pháp:**  
Giả sử chúng tôi có một dạng cấu trúc dữ liệu có thể đáp ứng hai kiểu truy vấn:
1\. Chèn một số X
2\. Truyền vào một Y, tìm XOR lớn nhất của Y với mọi số mà đã được thêm vào trước đó.

Nếu chúng ta có dạng cấu trúc dữ liệu này, chúng ta sẽ chèn các số nguyên khi chúng ta thực hiện và với kiểu câu truy vấn thứ 2 chúng ta sẽ tìm giá trị XOR lớn nhất.
Trie là kiểu cấu trúc dữ liệu mà chúng ta sẽ sử dụng. Đầu tiên, hãy tìm hiểu xem chúng là chèn các phần tử vào trie như thế nào.

![][2]

  
Vì vậy, chúng ta theo dõi đường dẫn của số mà chúng ta phải chèn vào, chúng ta không cần phải vẽ lại đường dẫn hiện có nữa. 
Để chèn một khoá có độ dài N lấy O (N) của log2(N) trong đó MAX là giá trị lớn nhất được thêm vào trie, vì chỉ có tối đa log2(MAX) bit nhị phân trên một số.
Đây là các chúng ta lưu trữ tất cả dữ liệu về tất cả các số được thêm vào trie cho tới giờ.
Bây giờ, để thực hiện truy vấn kiểu thứ 2:

Giả sử số Y của chúng ra là b1,b2,...bn trong đó  b1,b2,... là các bit nhị phân. Chúng ta bắt đầu từ b1. Bây giờ để XOR có thể lớn nhất, chúng ta sẽ cố gắng tạo ra một bit 1 (bit có giá trị là 1) quan trọng nhất sau khi lấy XOR. vì vậy nếu b1 có giá trị 0, chúng ta sẽ cần một bit 1 và ngược lại. Trên một trie, chúng ta đi đến phía bit được yêu cầu. Nếu không có tuỳ chọn thuận lợi, chúng ta sẽ đi hướng khác. Làm việc này lặp đi lặp lại từ i=1 tới i=n, chúng ta sẽ lấy được giá trị lớn nhất có thể của XOR.

![][3]

Câu truy vấn có thể là log2(MAX).

**Problem2**: Truyền vào một mảng các số nguyên, tìm một mảng con với XOR lớn nhất.
**Solution:**  
Giả sử F(L,R) là một XOR của mảng con từ L tới R.
Ở đây chúng ta sử dụng đặc tính là F(L,R) = F(1,R) XOR F(1, L-1). Vậy phải làm như nào?
Giả sử mảng con của chúng ta với một giá trị XOR lớn nhất được kết thúc tại vị trí i. Bây giờ chúng ta cần tối đa hoá F(L,i). ví dụ F(1,i) XOR F(1,L-1) với L<=i. Giả sử chúng ta thêm F(1,L-1) vào trie với mọi L<-i, thì đó chỉ là vấn đề đầu tiên.
    
    
    ans=0
    pre=0
    Trie.insert(0)
    for i=1 to N:
        pre = pre XOR a[i]
        Trie.insert(pre)
        ans=max(ans, Trie.query(pre))
    print ans
    

Bạn có thể thử vấn đề này ở đây: [ACM-ICPC Live Archive][4]

**Vấn đề 3**: Với một mảng các số nguyên dương, bạn phải in ra số lương các mảng con có XOR nhỏ hơn số K.

**Giải pháp:**  
Điều này lại một lần nữa sử dụng các khái niệm chúng ta đã tìm hiểu từ trước tới giờ. Chúng ta sẽ thực hiện như câu hỏi trước. 
Đối với mỗi chỉ số i=1 đến N, chúng ta có thể đếm được có bao nhiêu mảng con kết thúc tại vị trí thứ i mà thoả mãn điều kiện đã cho.

    
    
    ans=0
    p=0
    for i=1 to N:
        q=p XOR A[i]
        ans += Trie.query(q,k)
        Trie.insert(q)
        p=q
    

  
query(q,k) trả về số lượng các số nguyên trong cấu trúc mà khi XOR nó với q trả về một số nguyên nhỏ hơn k.
Chúng ta so sánh các bit tương ứng của q và k, bắt đầu từ các bit quan trọng nhất. Giả sử là p và q là các bit tương ứng mà chúng ta đang xem xét.

Nếu q bằng 1, p bằng 0 thì chúng ta thực hiện việc này:

![][5]

Tương tự chúng ta có thể dễ dàng thao tác với 3 trường hợp. (q=1,p=1), (q=0,p=1) and (q=0,p=1).

Vì vậy chúng ta cần thay đổi cấu trúc của mình ở đây, chúng ta cũng phải giữ số lượng node lá có thể truy cập từ node hiện tại nếu tôi đi từ phía bên trái và tương tự với phía bên phải. Bởi vì, nếu không làm vậy, sự phức tạp sẽ tăng lên, nếu chúng ta cứ lặp đi lặp lại việc duyệt cả cây.


Vấn đề này được nhắc đến trong CodeCraft'14. Bạn có thể làm thử lại đây: [SPOJ.com - Problem SUBXOR][6]

Bây giờ, hãy nói về việc triển khai nó.
Để triển khai một trie trên C/CPP, chúng ta có thể giữ các node và các con trỏ trái, phải. Chúng ta có thể viết các hàm đệ quy như này:
    
    
    insert(root, num, level):
        if level==-1: return root
        x=level'th bit of num
        if x==1:
            if root->right is NULL: create root->right
            else: insert(root->right,num,level-1)
        else:
            if root->left is NULL: create root->left
            else: insert(root->left,num,level-1)
    

  
Đối với các truy vấn tương tự, chúng ta có thể đệ truy trên toàn bộ cây.

cập nhật:
CÁc vấn đề khác khi sử dụng Trie(yay! :P)
[Problem - B - Codeforces][7]

**Các vấn đề phụ**: cho một nhóm các string không rỗng có _n_ phần tử. Trong trò chơi hai người cùng xây dựng một từ, bắt đầu từ một từ rỗng. Các người chơi đi qua các lượt. Trong lượt của người chơi phải thêm một chữ cái tốt nhất vào cuối của từ đó, và từ kết quả phải là tiền tố của ít nhất một trong các string thuộc nhóm đó. Người chơi sẽ thua nếu họ không thể tiếp tục.

Chúng ta cần tìm người chơi nào (thứ nhất hoặc thứ 2) có chiến lược để chiến thắng.

Vì vậy, ý tưởng ở đây là xây dựng lại một trie cho tất cả các chuỗi. Vì sao? Bởi vì một trie có thể lưu trữ thông tin về một tiền tố. 
Bây giờ chúng ta sẽ thử đánh giá mỗi node nếu là người chơi đầu tiên thì có chiến lược chiến thắng hay không. Chúng ta có thể thực hiện việc này bằng đệ quy. Ứng với node v, với mỗi node u sao cho u là node con gần nhất của v, nếu người chơi đầu tiên có chiến lược thua với nút v, thì đối với node v người chơi đầu tiên sẽ có chiến lược thắng.
Để ví dụ, chúng ta có "abc", "abd","acd".
Trie của chúng ta sẽ trông như này:

![][8]

  
Tất cả các node lá đều có chiến lược để chiến thắng.

[1]: https://qph.fs.quoracdn.net/main-qimg-aea28d9cd34aaf2d5783f4cd04e5abbd
[2]: https://qph.fs.quoracdn.net/main-qimg-388217a1992f1b2aac51e9917aa76d9c
[3]: https://qph.fs.quoracdn.net/main-qimg-e5d624e2cd693d713840a30ca9aaa461
[4]: https://icpcarchive.ecs.baylor.edu/index.php?Itemid=8&category=345&option=com_onlinejudge&page=show_problem&problem=2683
[5]: https://qph.fs.quoracdn.net/main-qimg-f24ea5ecf11805e7bcd82a48bb9cad25
[6]: http://www.spoj.com/problems/SUBXOR
[7]: http://codeforces.com/contest/455/problem/B
[8]: https://qph.fs.quoracdn.net/main-qimg-f81def67dffcc9e95306d65b27daa2f7-c

  