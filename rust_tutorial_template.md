# Rust Tutorial Project — Principles of Programming Languages

> **สำหรับนักศึกษา:** ใช้ไฟล์นี้เป็น Template สำหรับจัดทำบทเรียน Rust ของกลุ่ม  
> **Topic No.:** `5`  
> **Topic Name:** `Expressions & Statements`  
> **Group No.:** `5`

---

## 1. Members

| # | Name | Student ID | GitHub Username | Main Responsibility |
|---|---|---|---|---|
| 1 | `คมสัน กลิ่นหอม` | `670710124` | `@670710124` | Concept + Code |
| 2 | `ฐิติพงศ์ ราชธานี` | `670710125` | `@670710125` | Code + Demo |
| 3 | `ณัฏฐ์ธเนศ กุนทรฐิติวัสส์` | `670710126` | `@670710126` | Rust vs Other Language + PPL |
| 4 | `ณัฐพงศ์ อวชัย` | `670710127` | `@670710127` | Exercises + Common Mistakes |

---

## 2. Learning Objectives

หลังจากศึกษา Topic นี้แล้ว ผู้เรียนสามารถ:

1. `[อธิบายแนวคิดสำคัญได้]`
2. `[เขียนโปรแกรม Rust ที่เกี่ยวข้องได้]`
3. `[วิเคราะห์พฤติกรรม/กฎของภาษาได้]`
4. `[เปรียบเทียบ Rust กับภาษาอื่นได้]`

---

## 3. Introduction

Rust เป็นภาษาที่เน้นการทำงานผ่าน **expressions** เป็นหลัก โดย expression คือส่วนของโค้ดที่สามารถถูกประเมินแล้วให้ค่า (value) หรือทำให้เกิดผลบางอย่างจากการทำงานได้ ขณะที่ **statements** มีหน้าที่หลักในการจัดวางและกำหนดลำดับการประเมิน expression ภายในโปรแกรม

การเข้าใจความแตกต่างระหว่าง expressions และ statements จะมีผลต่อวิธีที่ Rust ใช้ประเมินค่านั้นๆ การใช้ semicolon (`;`) การกำหนดค่าของ block และการคืนค่าจาก function โดยเฉพาะใน Rust ที่ block, `if`, `match` และโครงสร้างอื่น ๆ หลายชนิดสามารถทำหน้าที่เป็น expression และให้ค่ากลับมาได้

ความเข้าใจในหัวข้อนี้ช่วยให้ผู้เขียนโปรแกรมสามารถอ่านและเขียนโค้ด Rust ได้ถูกต้องมากขึ้น เช่น รู้ว่าเมื่อใดค่าของ expression จะถูกนำไปใช้ เมื่อใดค่าจะถูกละทิ้ง และเหตุใดการเพิ่มหรือลบ semicolon บางตำแหน่งจึงอาจเปลี่ยนค่าหรือชนิดข้อมูลของ block ได้]`

---

## 4. Key Concepts

### 4.1 `Expression and Value`

**คำอธิบาย**

`Expression` คือส่วนของโค้ดที่เมื่อถูกประเมิน(evaluate) แล้วจะให้ค่า(value) ออกมา
`Rust` เป็นภาษาที่เน้น Expression เป็นหลัก โดย Expression หนึ่งสามารถเป็นส่วนย่อยของ Expression ที่ใหญ่กว่าได้

**ตัวอย่าง**

```rust
fn main() {
    let x = (5 + 3) * 2;
    println!("{}", x);
}
```

**Explanation**

`(5 + 3) * 2` คือ Expressionใหญ่ `5 + 3` คือ Expressionย่อย ซึ่งเมื่อถูก evaluate แล้วจะได้ value คือ 16
จากนั้นค่า 16 ถูกนำไปใช้เป็นค่าเริ่มต้นของตัวแปร x

---

### 4.2 `Statement`

**คำอธิบาย**

Statement เป็นองค์ประกอบที่อยู่ภายใน `block` และมีหน้าที่หลักในการจัดลำดับการทำงานของโปรแกรม

Rust แบ่ง Statement หลัก ๆ เป็น 2 ประเภท
1. `Declaration Statement` — ใช้ประกาศชื่อใหม่ เช่น ตัวแปรหรือ item
2. `Expression Statement` — ประเมิน Expression แล้วไม่ใช้ค่าผลลัพธ์ต่อ
ตัวอย่าง
```rust
fn main() {
    let x = 10;
    println!("{}", x);
}
```
**Explanation**
```rust
let x = 10;
```

let statement เป็น `Declaration Statement`
ภายใน statement นี้ 10 เป็น Expression ที่ให้ value 10 และค่านั้นถูกใช้เป็นค่าเริ่มต้นของ x

### 4.3 `Expression Statement`

**คำอธิบาย**

Expression Statement คือการนำ Expression มาประเมิน แต่ไม่ได้ใช้ค่าผลลัพธ์ที่ Expression คืนมา
โดยทั่วไปเราใช้ Expression Statement เมื่อต้องการผลจากการทำงาน (effect) ของ Expression มากกว่าค่าที่มันคืนมา

**ตัวอย่าง**
```rust
fn main() {
    let mut numbers = vec![1, 2, 3];

    numbers.pop();

    println!("{:?}", numbers);
}
```
**Explanation**

`numbers.pop()` จะนำสมาชิกตัวสุดท้ายออกจาก vector และคืนค่าของสมาชิกที่ถูกนำออก
ในตัวอย่างนี้ เราไม่ได้เก็บค่าที่ `pop()` คืนมา ดังนั้นค่าผลลัพธ์ถูกละทิ้ง แต่ effect ของการเรียก `pop()` ยังคงเกิดขึ้น

---

### 4.4 `Block Expression and Tail Expression`

**คำอธิบาย**

ใน Rust block `{ ... }` สามารถเป็น Expression และมี value ของตัวเองได้
ถ้า Expression ตัวสุดท้ายของ block ไม่มี semicolon `(;)` ค่าของ Expression นั้นจะกลายเป็นค่าของ block เราเรียก Expression ตำแหน่งนี้ว่า **Tail Expression**


**ตัวอย่าง**

```rust
fn main() {
    let result = {
        let a = 5;
        let b = 3;

        a + b
    };

    println!("{}", result);
}
```

ภายใน block มี Statement สองบรรทัด

```rust
let a = 5;
let b = 3;
```

ส่วน

```rust
a + b
```

เป็น `Tail Expression` เพราะเป็น Expression ตัวสุดท้ายและไม่มี `;`
มันให้ value `8` ดังนั้น block ทั้งก้อนจึงมี value เป็น `8` และ `result` จะมีค่าเท่ากับ `8`

---

### 4.5 `Unit Type ()`

**คำอธิบาย**

ถ้า block ไม่มี Tail Expression ที่ให้ค่าข้อมูลออกมา block จะมีค่าเป็น `()` ซึ่งเรียกว่า Unit value และมี type เป็น `()`
Unit ไม่ใช่ `null` แต่เป็นค่าที่ใช้แทนกรณีที่การทำงานเสร็จสิ้นโดยไม่มีข้อมูลที่มีความหมายให้ส่งออกมา

**ตัวอย่าง**

```rust
fn main() {
    let result = {
        5 + 3;
    };

    println!("{:?}", result);
}
```
`5 + 3` ยังคงถูก evaluate และได้ value `8`
แต่เนื่องจากมี `;`

```rust
5 + 3;
```
มันถูกใช้เป็น Expression Statement และค่า `8` ไม่ถูกใช้เป็นค่าของ block
ดังนั้น block นี้จึงมีค่าเป็น

```rust
()
```
และ result มี type เป็น '()'

---

## 5. Important Syntax / Rules

| Syntax / Rule | Meaning | Example |
|---|---|---|
| `let pattern = expression;` | `ใช้ประกาศตัวแปร และใช้ค่าจาก Expression เป็นค่าเริ่มต้น` | `let x = 5 + 3;` |
| `expression;` | `ใช้ Expression เป็น Expression Statement โดยประเมิน Expression แต่ไม่ใช้ค่าผลลัพธ์ต่อ` | `v.pop();` |
| `{ statements; expression }` | `Block Expression ที่มี Expression สุดท้ายเป็นค่าของ block` | `{ let x = 5; x + 1 }` |

### Important Rules

1. `Expression ที่ถูกใช้เป็น Expression Statement จะถูก evaluate แต่ค่าผลลัพธ์จะไม่ถูกนำไปใช้ต่อ`
2. `ExpressionWithoutBlock เมื่อนำมาใช้เป็น Expression Statement ต้องมี semicolon (;) ปิดท้าย`
```rust
v.pop();   // ต้องมี ;
5 + 3;     // ต้องมี ;
```
3. `ExpressionWithBlock สามารถละ semicolon (;) ได้เมื่อใช้เป็น Statement แต่ถ้าละ semicolon ผลลัพธ์ของ Expression นั้นต้องมี type เป็น Unit ()`
```rust
if v.is_empty() {
    v.push(5);
} else {
    v.remove(0);
}
```
`ตรงนี้ไม่ต้องมี ; หลัง } ก็ได้ เพราะเป็น ExpressionWithBlock และผลลัพธ์เป็น ()`

---

## 6. Runnable Code Examples

> **ข้อกำหนด:** Code ทุกตัวต้อง Compile และ Run ได้จริงก่อนนำมาใส่ในเอกสาร

### Example 1 — `การคำนวณเกรดด้วย Block Expression`

**Purpose:** `สาธิตการใช้ Block Expression ในการคืนค่า (Return Value) เข้าสู่ตัวแปรโดยตรงโดยไม่ต้องใช้คำสั่ง return และแสดงความแตกต่างระหว่างการลงท้ายด้วย Expression (ไม่มี ;) กับ Statement (มี ;)`

```rust
fn main() {
    let score = 85;

    // Block Expression: คืนค่า String slice เข้าตัวแปร grade โดยตรง
    let grade = {
        let bonus = 5;
        let total_score = score + bonus;

        // ไม่ใส่ Semicolon (;) เพื่อให้เป็น Expression คืนค่าออกไป
        if total_score >= 80 {
            "A"
        } else if total_score >= 70 {
            "B"
        } else {
            "F"
        }
    };

    println!("Total calculated grade: {}", grade);
}
```

**Expected Output**

```text
Total calculated grade: A
```

**Explanation**

```
1. let score = 85; เป็น Statement (Declaration Statement) สำหรับประกาศตัวแปร
2. let grade = { ... }; เป็นการนำ Block Expression มากำหนดค่าให้ตัวแปร grade
3. ตัวแปร bonus และ total_score เป็น Local Variables ที่อยู่ภายใน Block Scope เท่านั้น ไม่สามารถเรียกใช้นอก {} ได้
4. บรรทัดสุดท้ายภายใน Block (if total_score >= 80 { ... }) ไม่มี Semicolon ; ทำให้ทำหน้าที่เป็น Expression ที่ถูกประเมินค่าและคืนค่าเป็น &str ออกมาให้กับตัวแปร grade
```

---

### Example 2 — `การคืนค่าจาก Loop ด้วยคำสั่ง break`

**Purpose:** `สาธิตการใช้ loop ในฐานะ Expression ที่สามารถประมวลผลการทำงานซ้ำ และคืนค่าผลลัพธ์กลับมาเข้าตัวแปรได้ทันทีผ่านคำสั่ง break value;`

```rust
fn main() {
    let mut counter = 0;

    // loop เป็น Expression ที่ส่งค่ากลับมาเข้าตัวแปร result ได้โดยตรง
    let result = loop {
        counter += 1;

        if counter == 3 {
            // คืนค่า counter * 10 ออกไปให้ตัวแปร result แล้วหยุด loop ทันที
            break counter * 10;
        }
    };

    println!("The result from loop execution is: {}", result);
}
```

**Expected Output**

```text
The result from loop execution is: 30
```

**Explanation**
```
1. ในภาษา Rust โครงสร้างควบคุมอย่าง loop ถือเป็น Expression ไม่ใช่แค่ Statement เหมือนภาษา C หรือ Java   
2. การใส่ค่าไว้หลังคำสั่ง break (เช่น break counter * 10;) เป็นการส่งผลลัพธ์ออกจาก Loop มายังตัวแปรที่รับค่าทันที   
3. ช่วยให้เขียนโค้ดกระชับขึ้น เพราะตัวแปร result จะได้รับค่าประมวลผลทันที โดยไม่ต้องสร้างตัวแปร mut เปล่าๆ ไว้นอก Loop ก่อน   
```

---

## 7. Common Mistakes

### Mistake 1 — `ใส่ Semicolon หลัง Expression สุดท้าย`

**Problem**

`การใส่ `;` หลัง expression สุดท้ายของ block โดยไม่ตั้งใจ จะทำให้ block นั้นไม่คืนค่าที่ต้องการ แต่จะมีค่าเป็น `()` แทน`

**Incorrect Code**

```rust
let x = {
    5 + 3;
};
```

**Correct Code**

```rust
let x = {
    5 + 3
};
```

**Why?**

`Rust ใช้ expression สุดท้ายของ block เป็นค่าที่ส่งออกจาก block ได้ แต่ expression นั้นต้องไม่มี `;` ต่อท้าย เพราะถ้ามี `;` Rust จะมองเป็น statement และ block จะมีค่าเป็น `()` แทน`
`() เป็นชนิดข้อมูลที่ใช้แทนกรณีที่ “ไม่มีค่าข้อมูลที่มีความหมายให้ส่งกลับ”`

---

### Mistake 2 — `ให้ค่าจาก if แต่ละ branch เป็นคนละชนิด`

**Problem**

`เมื่อใช้ if เป็น expression ค่าที่ได้จากแต่ละ branch ต้องมีชนิดข้อมูลที่เข้ากันได้`

**Incorrect Code**

```rust
let condition = true;

let result = if condition {
    10
} else {
    "ten"
};
```

**Correct Code**

```rust
let condition = true;

let result = if condition {
    10
} else {
    20
};
```

**Why?**

`Rust สามารถใช้ if เป็น expression เพื่อสร้างค่าได้ แต่ค่าที่ได้จาก if ต้องมี type เดียวกัน`

---

## 8. Exercises

> จัดทำแบบฝึกหัด **2 ข้อ** ที่สอดคล้องกับ Topic และมีระดับความยากเหมาะสม

### Exercise 1 — `Statement หรือ Expression`

**Problem**

`จงระบุว่าแต่ละบรรทัดเป็น Statement หรือ Expression`

```rust
let x = 10;
x + 5
x + 5;
if x > 5 { 1 } else { 0 }
```

**Hint**

`Expression สร้างค่า ส่วน statement ใช้ทำงานบางอย่างและมักจบด้วย ;`

**Solution**

```rust
let x = 10;                  -> Statement
x + 5                       -> Expression
x + 5;                      -> Expression ที่ถูกใช้เป็น statement
if x > 5 { 1 } else { 0 }   -> Expression
```

**Explanation**

`ใน Rust expression คือโค้ดที่ให้ค่าออกมา เช่น `x + 5` หรือ `if ... { ... } else { ... }``

`ส่วน statement คือคำสั่งที่ใช้ทำงานบางอย่าง เช่น `let x = 10;``

`เมื่อเติม `;` หลัง expression เช่น `x + 5;` ค่าที่ได้จาก expression จะไม่ถูกนำไปใช้ต่อ และ expression นั้นจะถูกใช้ในรูปของ statement`

---

### Exercise 2 — `[ใช้ if Expression เพื่อสร้างค่า]`

**Problem**

จงเติมโค้ดให้ตัวแปร `grade` มีค่าเป็น

- `"A"` เมื่อ `score >= 80`
- `"B"` เมื่อ `score >= 70`
- `"C"` ในกรณีอื่น

```rust
let score = 75;

let grade = ??????????;



println!("{}", grade);
```

**Hint**

`Rust สามารถใช้ if และ else if เป็น expression เพื่อสร้างค่าได้`

**Solution**

```rust
let score = 75;

let grade = if score >= 80 {
    "A"
} else if score >= 70 {
    "B"
} else {
    "C"
};

println!("{}", grade);
```

**Explanation**

`Rust สามารถใช้ if เป็น expression ได้ โดยค่าจาก branch ที่ตรงกับเงื่อนไขจะกลายเป็นค่าของ expression และถูกนำไปเก็บใน grade เมื่อ score = 75 เงื่อนไข score >= 70 เป็นจริง จึงได้ค่า "B"`

---

## 9. PPL Perspective

> **ส่วนนี้เป็นหัวใจของรายวิชา Principles of Programming Languages**

วิเคราะห์ Topic นี้ในมุมมองของ Programming Languages

### 9.1 Syntax

`[Topic นี้เกี่ยวข้องกับ syntax อย่างไร]`

### 9.2 Semantics

`[คำสั่ง/construct เหล่านี้มีความหมายหรือพฤติกรรมอย่างไร]`

### 9.3 Type System

`[เกี่ยวข้องกับ type system อย่างไร ถ้ามี]`

### 9.4 Memory / Resource Management

`[เกี่ยวข้องกับ memory หรือ resource management อย่างไร ถ้ามี]`

### 9.5 Abstraction / Other PPL Concepts

`[อธิบาย abstraction, scope, binding, paradigm หรือแนวคิด PPL อื่นที่เกี่ยวข้อง]`

### 9.6 Why Rust?

`[Rust ใช้แนวคิดนี้เพื่อเพิ่ม safety, reliability หรือ performance อย่างไร]`

---

## 10. Rust vs. Other Language

**Comparison Language:** `[Python / C / C++ / Java / Kotlin / ...]`

| Aspect | Rust | Other Language |
|---|---|---|
| Syntax | `[อธิบาย]` | `[อธิบาย]` |
| Semantics / Behavior | `[อธิบาย]` | `[อธิบาย]` |
| Type System | `[อธิบาย]` | `[อธิบาย]` |
| Memory Management | `[อธิบาย]` | `[อธิบาย]` |
| Safety | `[อธิบาย]` | `[อธิบาย]` |

### Rust Example

```rust
// Rust code
```

### `[Other Language]` Example

```python
# Other language code
```

### Analysis

`[อธิบายความแตกต่างที่สำคัญ และเหตุผลด้านการออกแบบภาษา]`

---

## 11. Teach Your Topic

การนำเสนอมีสมาชิก **4 คน คนละประมาณ 5 นาที**

| Member | Responsibility | Time |
|---|---|---:|
| Member 1 | Concept + Short Code Illustration | 5 min |
| Member 2 | Detailed Code + Live Demo | 5 min |
| Member 3 | Rust vs Other Language + PPL Analysis | 5 min |
| Member 4 | Exercises + Common Mistakes + Challenge | 5 min |

### Individual Contribution

**Member 1**

`[สิ่งที่รับผิดชอบ]`

**Member 2**

`[สิ่งที่รับผิดชอบ]`

**Member 3**

`[สิ่งที่รับผิดชอบ]`

**Member 4**

`[สิ่งที่รับผิดชอบ]`

> สมาชิกทุกคนต้องสามารถอธิบาย Code ของกลุ่มได้ ไม่ใช่เฉพาะส่วนที่ตนเองเขียน

---

## 12. References

> แนะนำให้มีอย่างน้อย **4 แหล่งอ้างอิง** และควรใช้เอกสารทางการเป็นหลัก

1. `[The Rust Programming Language — Rust Book]`
2. `[Rust by Example / Rust Reference]`
3. `[Official documentation ที่เกี่ยวข้องกับ Topic]`
4. `[แหล่งอ้างอิงเพิ่มเติม]`

---

## 13. AI Usage Declaration

สามารถใช้ AI เป็นเครื่องมือช่วยเรียนรู้และพัฒนาได้ แต่สมาชิกทุกคนต้องเข้าใจและสามารถอธิบายผลงานของกลุ่มได้

| AI Tool | Purpose | How the Result Was Verified |
|---|---|---|
| `ChatGPT` | `หาข้อมูล, เปรียบเทียบหัวข้อแล้วให้สรุป` | `ใช้ file book จากแหล่งต่างๆเทียบ` |

### Declaration

- [ success ] Code ทุกส่วนที่นำเสนอได้รับการ Compile และทดสอบแล้ว
- [ ] สมาชิกทุกคนสามารถอธิบาย Code ที่นำเสนอได้
- [ ] ตรวจสอบข้อมูลจากแหล่งอ้างอิงที่น่าเชื่อถือแล้ว
- [ ] ระบุการใช้ AI อย่างโปร่งใส

**รายละเอียดการใช้ AI**

`[อธิบายว่าใช้ AI ในขั้นตอนใด และสมาชิกตรวจสอบผลลัพธ์อย่างไร]`
ใช้ AI ในขึ้นตอน เทียบเนื้อหาหัวข้อ, เจน code ต่างๆ และตรวจสอบผ่านการรันโค้ตบน virtual studio code กับ book file
---

## 14. GitHub Contribution

| Member | Issues | Commits | Pull Requests | Code Reviews | Contribution |
|---|---:|---:|---:|---:|---|
| Member 1 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 2 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 3 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 4 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |

### Teamwork Reflection

**How did your team collaborate?**

`[อธิบายกระบวนการทำงานร่วมกัน]`

**Problems encountered**

`[ปัญหาที่พบ]`

**How did you solve them?**

`[วิธีแก้ปัญหา]`

---

## 15. Final Checklist

- [ ] Learning Objectives ครบ 3–4 ข้อ
- [ ] Key Concepts ครบถ้วน
- [ ] Syntax / Rules
- [ ] Runnable Code Examples
- [ ] Code Compile และ Run ได้จริง
- [ ] Common Mistakes
- [ ] Exercises 2 ข้อ พร้อม Solutions
- [ ] PPL Perspective
- [ ] Rust vs Other Language
- [ ] References อย่างน้อย 4 แหล่ง
- [ ] AI Usage Declaration
- [ ] GitHub Contribution
- [ ] สมาชิกทั้ง 4 คนมีส่วนร่วม
- [ ] สมาชิกทั้ง 4 คนพร้อมนำเสนอคนละ 5 นาที
- [ ] สมาชิกทุกคนสามารถอธิบาย Code ของกลุ่มได้

---

## Submission Information

**Repository:** `https://github.com/670710127/Rust-Expression-Statements`

**Chapter Path:** `[เช่น chapters/01-introduction/]`

**Final PR:** `#[PR number]`

**Submitted by:** `[Group XX]`

**Date:** `[YYYY-MM-DD]`
