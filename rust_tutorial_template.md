# Rust Tutorial Project — Principles of Programming Languages

> **สำหรับนักศึกษา:** ใช้ไฟล์นี้เป็น Template สำหรับจัดทำบทเรียน Rust ของกลุ่ม  
> **Topic No.:** `XX`  
> **Topic Name:** `[ชื่อหัวข้อ]`  
> **Group No.:** `XX`

---

## 1. Members

| # | Name | Student ID | GitHub Username | Main Responsibility |
|---|---|---|---|---|
| 1 | `คมสัน กลิ่นหอม` | `670710124` | `@670710124` | Concept + Code |
| 2 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Code + Demo |
| 3 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Rust vs Other Language + PPL |
| 4 | `[ชื่อ-นามสกุล]` | `[รหัส]` | `@[username]` | Exercises + Common Mistakes |

---

## 2. Learning Objectives

หลังจากศึกษา Topic นี้แล้ว ผู้เรียนสามารถ:

1. อธิบายความแตกต่างระหว่าง **Expression** และ **Statement** ใน Rust ได้
2. อธิบายการทำงานของ **Block Expression**, **Tail Expression** และการคืนค่า (return value) จาก block/function ได้
3. เขียนและวิเคราะห์การใช้ `if` และ `match` ในฐานะ Expression ได้
4. เปรียบเทียบแนวคิด Expressions & Statements ของ Rust กับภาษา Python และ C ได้

---

## 3. Introduction

Rust เป็นภาษาที่เน้นการทำงานผ่าน **expressions** เป็นหลัก โดย Expression คือส่วนของโค้ดที่เมื่อถูกประเมิน (evaluate) แล้วจะให้ค่า (value) และในระหว่างการประเมินอาจทำให้เกิดผลจากการทำงาน (effect) ได้ ส่วน **Statement** ใช้สำหรับประกาศสิ่งต่าง ๆ หรือจัดลำดับการประเมิน Expression ภายใน block

การเข้าใจ Expressions & Statements มีความสำคัญ เพราะช่วยให้เข้าใจว่าโค้ดส่วนใดสร้างค่า โค้ดส่วนใดนำค่านั้นไปใช้ต่อ และ semicolon (`;`) มีผลต่อค่าของ block อย่างไร แนวคิดนี้เชื่อมโดยตรงกับ **Block Expression**, การคืนค่าจาก function และการใช้ `if` / `match` เป็น Expression

---

## 4. Key Concepts

### 4.1 Expression vs Statement

**Expression** คือส่วนของโค้ดที่ถูก evaluate แล้วให้ value

```rust
5 + 3
```

`5 + 3` เป็น Expression และให้ value เป็น `8`

**Statement** เป็นองค์ประกอบภายใน block ที่ใช้ประกาศสิ่งต่าง ๆ หรือจัดลำดับการทำงาน

```rust
let x = 5 + 3;
```

ในบรรทัดนี้:

```text
let x = 5 + 3;  → Statement
        5 + 3   → Expression
          8     → Value
```

Rust มี Statement หลัก ๆ 2 กลุ่ม:

- **Declaration Statement** เช่น `let x = 10;`
- **Expression Statement** เช่น `v.pop();` ซึ่ง evaluate Expression แต่ไม่ใช้ค่าผลลัพธ์ต่อ

ตัวอย่าง Expression Statement:

```rust
let mut v = vec![1, 2, 3];
v.pop();
```

`v.pop()` นำสมาชิกตัวท้ายออกจาก vector และคืนค่ากลับมา แต่ในตัวอย่างนี้ค่าที่คืนมาจะไม่ถูกนำไปใช้ต่อ

---

### 4.2 Block Expression

ใน Rust block `{ ... }` สามารถเป็น Expression และมี value ของตัวเองได้

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

`a + b` เป็น Expression สุดท้ายของ block และไม่มี `;` จึงทำให้ block มี value เป็น `8`

---

### 4.3 Tail Expression and Return Value

Expression สุดท้ายของ block ที่ไม่มี semicolon (`;`) เรียกว่า **Tail Expression** และค่าของมันจะกลายเป็นค่าของ block

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

`a + b` เป็น Tail Expression ดังนั้นค่าที่ได้จะถูกใช้เป็น return value ของ function โดยไม่จำเป็นต้องเขียน `return`

ถ้าเขียน:

```rust
let x = {
    5 + 3;
};
```

`5 + 3;` ถูกใช้เป็น Expression Statement ค่าที่ได้จะไม่ถูกใช้เป็นค่าของ block และเมื่อ block จบการทำงานตามปกติโดยไม่มี Tail Expression block จะมีค่าเป็น Unit `()`

---

### 4.4 `if` as an Expression

ใน Rust `if` สามารถเป็น Expression และให้ value ได้

```rust
fn main() {
    let score = 75;

    let grade = if score >= 80 {
        "A"
    } else if score >= 70 {
        "B"
    } else {
        "C"
    };

    println!("{}", grade);
}
```

เมื่อ `score = 75` ค่า Expression ของ `if` คือ `"B"` และค่านี้ถูกนำไปกำหนดให้ `grade`

เมื่อใช้ `if` เพื่อสร้างค่า แต่ละ branch ต้องให้ค่าที่มีชนิดข้อมูลเข้ากันได้

---

### 4.5 `match` as an Expression

`match` สามารถเป็น Expression และให้ value จาก arm ที่ match ได้

```rust
fn main() {
    let number = 2;

    let text = match number {
        1 => "One",
        2 => "Two",
        _ => "Other",
    };

    println!("{}", text);
}
```

เมื่อ `number = 2` arm ที่ตรงคือ `2 => "Two"` ดังนั้น `match` ทั้งก้อนมี value เป็น `"Two"`

---

## 5. Important Syntax / Rules

| Syntax / Rule | Meaning | Example |
|---|---|---|
| `let pattern = expression;` | ประกาศตัวแปร และใช้ค่าจาก Expression เป็นค่าเริ่มต้น | `let x = 5 + 3;` |
| `expression;` | ใช้ Expression เป็น Expression Statement และไม่ใช้ค่าผลลัพธ์ต่อ | `v.pop();` |
| `{ ... final_expression }` | Block Expression ที่ใช้ค่าจาก Expression สุดท้ายเป็นค่าของ block | `{ let x = 5; x + 1 }` |
| `if ... { expr } else { expr }` | `if` สามารถสร้าง value ได้ | `let x = if c { 1 } else { 0 };` |
| `match value { ... }` | `match` สามารถสร้าง value จาก arm ที่ตรงได้ | `let x = match n { 1 => "A", _ => "B" };` |

### Important Rules

1. Expression Statement จะ evaluate Expression แต่ไม่ใช้ค่าผลลัพธ์ต่อ
2. Expression สุดท้ายของ block ที่ไม่มี `;` จะเป็น Tail Expression และค่าของมันจะกลายเป็นค่าของ block
3. ถ้า block ไม่มี Tail Expression และจบการทำงานตามปกติ block จะมีค่าเป็น `()`
4. `if` ที่ใช้เป็น Expression ต้องให้ค่าจากแต่ละ branch ที่มี type เข้ากันได้
5. `match` ที่ใช้เป็น Expression จะให้ value จาก arm ที่ถูกเลือก

---
## 6. Runnable Code Examples

> **ข้อกำหนด:** Code ทุกตัวต้อง Compile และ Run ได้จริงก่อนนำมาใส่ในเอกสาร

### Example 1 — `[ชื่อ Example]`

**Purpose:** `[ต้องการสาธิตอะไร]`

```rust
fn main() {
    // Write your runnable Rust code here
}
```

**Expected Output**

```text
[expected output]
```

**Explanation**

`[อธิบาย code ทีละส่วนที่สำคัญ]`

---

### Example 2 — `[ชื่อ Example]`

**Purpose:** `[ต้องการสาธิตอะไร]`

```rust
fn main() {
    // Write your runnable Rust code here
}
```

**Expected Output**

```text
[expected output]
```

**Explanation**

`[อธิบาย code]`

---

## 7. Common Mistakes

### Mistake 1 — `[ชื่อข้อผิดพลาด]`

**Problem**

`[อธิบายปัญหา]`

**Incorrect Code**

```rust
// Incorrect example
```

**Correct Code**

```rust
// Correct example
```

**Why?**

`[อธิบายสาเหตุ]`

---

### Mistake 2 — `[ชื่อข้อผิดพลาด]`

**Problem**

`[อธิบายปัญหา]`

**Incorrect Code**

```rust
// Incorrect example
```

**Correct Code**

```rust
// Correct example
```

**Why?**

`[อธิบายสาเหตุ]`

---

## 8. Exercises

> จัดทำแบบฝึกหัด **2 ข้อ** ที่สอดคล้องกับ Topic และมีระดับความยากเหมาะสม

### Exercise 1 — `[ชื่อโจทย์]`

**Problem**

`[เขียนโจทย์]`

**Hint**

`[คำใบ้]`

**Solution**

```rust
// Solution code
```

**Explanation**

`[อธิบายแนวทางแก้]`

---

### Exercise 2 — `[ชื่อโจทย์]`

**Problem**

`[เขียนโจทย์]`

**Hint**

`[คำใบ้]`

**Solution**

```rust
// Solution code
```

**Explanation**

`[อธิบายแนวทางแก้]`

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
| `[เช่น ChatGPT]` | `[ใช้เพื่ออะไร]` | `[ตรวจสอบอย่างไร]` |
| `[AI tool]` | `[ใช้เพื่ออะไร]` | `[ตรวจสอบอย่างไร]` |

### Declaration

- [ ] Code ทุกส่วนที่นำเสนอได้รับการ Compile และทดสอบแล้ว
- [ ] สมาชิกทุกคนสามารถอธิบาย Code ที่นำเสนอได้
- [ ] ตรวจสอบข้อมูลจากแหล่งอ้างอิงที่น่าเชื่อถือแล้ว
- [ ] ระบุการใช้ AI อย่างโปร่งใส

**รายละเอียดการใช้ AI**

`[อธิบายว่าใช้ AI ในขั้นตอนใด และสมาชิกตรวจสอบผลลัพธ์อย่างไร]`

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

**Repository:** `[GitHub repository URL]`

**Chapter Path:** `[เช่น chapters/01-introduction/]`

**Final PR:** `#[PR number]`

**Submitted by:** `[Group XX]`

**Date:** `[YYYY-MM-DD]`
