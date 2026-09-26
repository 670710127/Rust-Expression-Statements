# Rust Tutorial Project — Principles of Programming Languages

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

### Example 1 — `if` และ Block Expression เพื่อสร้างค่า

**Purpose:** สาธิต Block Expression, Tail Expression และ `if` ในฐานะ Expression

```rust
fn main() {
    let score = 85;

    let grade = {
        let bonus = 5;
        let total_score = score + bonus;

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

---

### Example 2 — `match` ในฐานะ Expression

**Purpose:** สาธิตการใช้ `match` เพื่อเลือกและคืนค่าโดยตรง

```rust
fn main() {
    let level = 2;

    let message = match level {
        1 => "Beginner",
        2 => "Intermediate",
        3 => "Advanced",
        _ => "Unknown",
    };

    println!("{}", message);
}
```

**Expected Output**

```text
Intermediate
```

---

## 7. Common Mistakes

### Mistake 1 — ใส่ Semicolon หลัง Tail Expression โดยไม่ตั้งใจ

**Incorrect**

```rust
let x = {
    5 + 3;
};
```

**Correct**

```rust
let x = {
    5 + 3
};
```

### Mistake 2 — ให้ค่าจาก `if` แต่ละ branch เป็นคนละ type

**Incorrect**

```rust
let condition = true;

let result = if condition {
    10
} else {
    "ten"
};
```

**Correct**

```rust
let condition = true;

let result = if condition {
    10
} else {
    20
};
```

---

## 8. Exercises

### Exercise 1 — Statement หรือ Expression

พิจารณาแต่ละบรรทัดแยกจากกัน แล้วระบุว่าเป็น Statement หรือ Expression

```rust
let x = 10;
```

```rust
x + 5
```

```rust
x + 5;
```

```rust
if x > 5 { 1 } else { 0 }
```

**Solution**

```text
let x = 10;                  → Statement
x + 5                        → Expression
x + 5;                       → Expression Statement
if x > 5 { 1 } else { 0 }    → Expression
```

### Exercise 2 — ใช้ `match` เพื่อสร้างค่า

```rust
let number = 2;

let text = match number {
    1 => "One",
    2 => "Two",
    _ => "Other",
};

println!("{}", text);
```

---

## 9. PPL Perspective

### 9.1 Syntax

Rust ใช้ syntax ของ block และ semicolon เพื่อกำหนดว่าค่าของ Expression จะถูกใช้ต่อหรือไม่

### 9.2 Semantics

```rust
{ 5 + 3 }   // value = 8
{ 5 + 3; }  // value = ()
```

### 9.3 Type System

เมื่อ `if` หรือ `match` ถูกใช้เป็น Expression ค่าที่ได้จากแต่ละ branch/arm ต้องสามารถมี type ที่เข้ากันได้

### 9.4 Scope

Block กำหนด scope ของ local variables

```rust
let result = {
    let x = 10;
    x + 5
};

// x ใช้นอก block นี้ไม่ได้
```

### 9.5 Expression-Oriented Design

Rust อนุญาตให้ block, `if` และ `match` สร้าง value ได้โดยตรง

---

## 10. Rust vs Other Languages

เพื่อให้เห็นลักษณะ Expression-oriented ของ Rust ชัดขึ้น จะเปรียบเทียบกับ **Python** และ **C** โดยเน้นเฉพาะหัวข้อที่อยู่ใน scope ของงานนี้

### 10.1 Comparison Overview

| Aspect | Rust | Python | C |
|---|---|---|---|
| Expression vs Statement | หลาย control-flow constructs เป็น Expression | มีทั้ง Expression และ Statement | มีทั้ง Expression และ Statement |
| Block มี value | ได้ | ไม่มี block value แบบ Rust | ไม่มี block value แบบ Rust |
| `if` สร้าง value โดยตรง | ได้ | `if` statement ไม่ให้ value โดยตรง; ถ้าต้องการ expression ใช้ conditional expression | `if` เป็น statement; ถ้าต้องการ expression ใช้ `?:` |
| `match` แบบ Rust | `match` เป็น Expression | `match` เป็น statement ไม่ใช่ value-producing expression แบบ Rust | ไม่มี `match`; โดยทั่วไปใช้ `switch` statement |
| Return value จากท้าย block | ได้ผ่าน Tail Expression | function ใช้ `return` | function ใช้ `return` |

### 10.2 `if` — Rust vs Python vs C

#### Rust

```rust
let grade = if score >= 80 {
    "A"
} else {
    "B"
};
```

#### Python

```python
grade = "A" if score >= 80 else "B"
```

#### C

```c
const char *grade = (score >= 80) ? "A" : "B";
```

**สรุป:** ทั้งสามภาษาสามารถเลือกค่าได้ แต่ Rust ทำให้ `if { ... } else { ... }` เองเป็น Expression ได้โดยตรง

### 10.3 `match` — Rust vs Python vs C

#### Rust

```rust
let text = match number {
    1 => "One",
    2 => "Two",
    _ => "Other",
};
```

#### Python

```python
match number:
    case 1:
        text = "One"
    case 2:
        text = "Two"
    case _:
        text = "Other"
```

#### C

```c
switch (number) {
    case 1:
        text = "One";
        break;
    case 2:
        text = "Two";
        break;
    default:
        text = "Other";
}
```

**สรุป:** Rust `match` ทั้งก้อนเป็น Expression จึงสามารถสร้าง value และวางทางขวาของ `=` ได้โดยตรง

### 10.4 Presentation Scope for Member 3 — ไม่เกิน 5 นาที

1. **45 วินาที** — ตารางภาพรวม Rust / Python / C
2. **1 นาที 30 วินาที** — เปรียบเทียบ `if`
3. **1 นาที 30 วินาที** — เปรียบเทียบ `match`
4. **45 วินาที** — สรุปว่า Rust มีแนวทาง Expression-oriented อย่างไร

ไม่ต้องอธิบาย memory management, ownership, borrowing หรือเรื่องนอก scope เพราะอาจารย์กำหนดให้เน้น expression vs statement, block expression, return value และ `if`/`match` ในฐานะ expression

---

## 11. Teach Your Topic

| Member | Responsibility | Time |
|---|---|---:|
| Member 1 | Expression vs Statement + Block / Tail / Return Value Concept | 5 min |
| Member 2 | Detailed Code + `if` / `match` Demo | 5 min |
| Member 3 | Rust vs Python/C + PPL Analysis | 5 min |
| Member 4 | Exercises + Common Mistakes + Challenge | 5 min |

---

## 12. References

1. [The Rust Reference — Statements and Expressions](https://doc.rust-lang.org/reference/statements-and-expressions.html)
2. [The Rust Reference — Statements](https://doc.rust-lang.org/reference/statements.html)
3. [The Rust Reference — Expressions](https://doc.rust-lang.org/reference/expressions.html)
4. [The Rust Reference — Block Expressions](https://doc.rust-lang.org/reference/expressions/block-expr.html)
5. [The Rust Reference — If Expressions](https://doc.rust-lang.org/reference/expressions/if-expr.html)
6. [The Rust Reference — Match Expressions](https://doc.rust-lang.org/reference/expressions/match-expr.html)
7. [Python Documentation — Conditional Expressions](https://docs.python.org/3/reference/expressions.html#conditional-expressions)
8. [Python Documentation — match statement](https://docs.python.org/3/reference/compound_stmts.html#the-match-statement)

---

## 13. AI Usage Declaration

| AI Tool | Purpose | How the Result Was Verified |
|---|---|---|
| `ChatGPT` | ช่วยจัดโครงสร้างเนื้อหา เปรียบเทียบหัวข้อ และสร้างตัวอย่าง code | ตรวจสอบกับ Rust Reference และทดลอง Compile/Run code ก่อนนำเสนอ |

---

## 14. GitHub Contribution

| Member | Issues | Commits | Pull Requests | Code Reviews | Contribution |
|---|---:|---:|---:|---:|---|
| Member 1 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 2 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 3 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |
| Member 4 | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[จำนวน]` | `[รายละเอียด]` |

---

## 15. Final Checklist

- [x] Expression vs Statement
- [x] Block Expression
- [x] Return Value / Tail Expression
- [x] `if` as Expression
- [x] `match` as Expression
- [x] Rust vs Python / C
- [ ] Compile และ Run code จริงครบทุกส่วน
- [ ] สมาชิกทั้ง 4 คนพร้อมนำเสนอคนละ 5 นาที

---

## Submission Information

**Repository:** `https://github.com/670710127/Rust-Expression-Statements`

**Chapter Path:** `[chapter path]`

**Final PR:** `#[PR number]`

**Submitted by:** `Group 5`

**Date:** `[YYYY-MM-DD]`
