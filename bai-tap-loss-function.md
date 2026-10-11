# Bài tập: SVM Loss và Softmax Loss

Quy ước: thứ tự class luôn là **[cat, car, frog]**. Margin của SVM = 1.

Công thức cần nhớ:

- **SVM (hinge loss):** `Li = Σ_{j≠yi} max(0, s_j − s_yi + 1)`
- **Softmax (cross-entropy):** `Li = −ln( e^(s_yi) / Σ_j e^(s_j) )`
- **Data loss trung bình:** `(1/N) Σ Li`

Hãy tự làm trước, rồi mới xem đáp án ở cuối file.

---

## Phần A: SVM loss

**Bài 1.** Class đúng là cat. Score: cat = 3.0, car = 4.0, frog = −2.0. Tính Li.

**Bài 2.** Class đúng là cat. Score: cat = 5.0, car = 3.5, frog = 4.2. Tính Li.
(Dự đoán của model đúng hay sai? Loss có bằng 0 không?)

**Bài 3.** Có 3 ảnh, mỗi ảnh có score và nhãn đúng:

| Ảnh | Score [cat, car, frog] | Nhãn đúng |
|-----|------------------------|-----------|
| A   | [3.2, 5.1, −1.7]       | cat       |
| B   | [1.3, 4.9, 2.0]        | car       |
| C   | [2.2, 2.5, −3.1]       | frog      |

Tính Li của từng ảnh, rồi tính data loss trung bình `(1/N) Σ Li`.

---

## Phần B: Softmax loss

**Bài 4.** Class đúng là cat. Score: [2, 1, 0].

- a) Tính `e^s` cho từng class.
- b) Tính xác suất softmax cho từng class.
- c) Tính `L = −ln(P(cat))`.

**Bài 5.** Ba class, score ban đầu đều bằng nhau: [1, 1, 1]. Tính softmax loss.
(Gợi ý: xác suất mỗi class là bao nhiêu?)

---

## Phần C: Hiểu bản chất

**Bài 6.** Khi mới khởi tạo, nếu W ≈ 0 thì mọi score ≈ 0. Với **C class**, loss ban đầu của SVM và của softmax xấp xỉ bao nhiêu?
(Đây là cách "sanity check" khi bắt đầu train.)

**Bài 7.** Nếu tăng score của class đúng lên **+100** (giữ nguyên các score khác), SVM loss và softmax loss thay đổi thế nào? Có loss nào về đúng 0 không?

**Bài 8.** Giá trị nhỏ nhất và lớn nhất có thể của mỗi loại loss là bao nhiêu?

---

## Đáp án

### Bài 1
- car: 4 − 3 + 1 = 2
- frog: −2 − 3 + 1 = −4 → max(0, −4) = 0
- **Li = 2**

### Bài 2
- car: 3.5 − 5 + 1 = −0.5 → 0
- frog: 4.2 − 5 + 1 = 0.2
- **Li = 0.2**

Dự đoán là **đúng** (cat có điểm cao nhất), nhưng frog chỉ kém cat 0.8 < margin 1 nên vẫn bị phạt nhẹ.

### Bài 3
- **A** (đúng là cat = 3.2): car: 5.1 − 3.2 + 1 = 2.9; frog: −1.7 − 3.2 + 1 = −3.9 → 0. **L_A = 2.9**
- **B** (đúng là car = 4.9): cat: 1.3 − 4.9 + 1 = −2.6 → 0; frog: 2.0 − 4.9 + 1 = −1.9 → 0. **L_B = 0**
- **C** (đúng là frog = −3.1): cat: 2.2 + 3.1 + 1 = 6.3; car: 2.5 + 3.1 + 1 = 6.6. **L_C = 12.9**
- **Trung bình** = (2.9 + 0 + 12.9) / 3 = 15.8 / 3 ≈ **5.27**

### Bài 4
- a) e² ≈ 7.389; e¹ ≈ 2.718; e⁰ = 1
- b) Tổng ≈ 11.107, nên P(cat) ≈ 0.665, P(car) ≈ 0.245, P(frog) ≈ 0.090
- c) **L = −ln(0.665) ≈ 0.41**

### Bài 5
Mỗi class có xác suất 1/3, nên **L = −ln(1/3) = ln 3 ≈ 1.10**.

### Bài 6
- **SVM:** mọi score bằng 0, mỗi class sai cho max(0, 0 − 0 + 1) = 1, nên **L ≈ C − 1**.
- **Softmax:** mỗi class có xác suất 1/C, nên **L ≈ ln C**.

Nếu lúc bắt đầu train mà loss lệch xa các giá trị này thì khả năng cao là code bị bug.

### Bài 7
- **SVM:** class đúng thắng rất xa nên mọi số hạng max(0, ·) = 0, **loss = 0 và dừng ở 0**.
- **Softmax:** xác suất class đúng tiến sát 1, loss tiến sát 0 nhưng **không bao giờ chạm 0 hẳn**.

Đây chính là khác biệt: SVM đạt margin là xong, softmax luôn muốn tự tin hơn.

### Bài 8
- **SVM:** nhỏ nhất **0**, lớn nhất **không giới hạn** (+∞ về lý thuyết).
- **Softmax:** nhỏ nhất **tiến tới 0** (không chạm), lớn nhất **+∞** (khi xác suất class đúng → 0).
