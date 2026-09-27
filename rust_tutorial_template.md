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

### 4.6 `loop` as an Expression

ใน Rust `loop` ก็สามารถเป็น **Expression** และให้ value ได้เช่นกัน

```rust
fn main() {
    let result = loop {
        break 10;
    };

    println!("{}", result);
}
```

ในตัวอย่างนี้ `loop` จะทำงานจนเจอ

```rust
break 10;
```

`break` จะหยุด `loop` และส่งค่า `10` ออกมาเป็น value ของ `loop` ทั้งก้อน ดังนั้น `result` จะมีค่าเป็น `10`

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

### Example 2 — `การคืนค่าด้วย match Expression และคำสั่ง return`

**Purpose:** `สาธิตการใช้ match ในฐานะ Expression เพื่อประเมินค่าผลลัพธ์ (Value) รวมถึงการใช้คำสั่ง return สำหรับการออกจากฟังก์ชันล่วงหน้า (Early Return) เมื่อเจอเงื่อนไขขอบเขต`

```rust
fn check_user_role(level: u32) -> &'static str {
    // Early Return: ใช้คำสั่ง return เพื่อคืนค่าและออกจากฟังก์ชันทันที
    if level == 0 {
        return "Guest";
    }

    // match ในฐานะ Expression: คืนค่า String slice ออกจากฟังก์ชันโดยไม่ต้องใช้คำสั่ง return
    match level {
        1 => "Member",
        2 => "Moderator",
        3 => "Admin",
        _ => "Unknown Role",
    }
}

fn main() {
    let user_level = 2;
    let role = check_user_role(user_level);

    println!("User role is: {}", role);
}
```

**Expected Output**

```text
User role is: Moderator
```

**Explanation**
```
1. คำสั่ง return (Explicit Return): บรรทัด return "Guest"; ใช้สำหรับหยุดการทำงานและส่งค่าออกจากฟังก์ชันทันทีก่อนจะไปถึงโค้ดส่วนอื่น (Early Return)  
2. match ในฐานะ Expression: โครงสร้าง match ทำหน้าที่ประเมินค่าและส่งผลลัพธ์จาก Arm ที่จับคู่สำเร็จออกมาเป็น Value เพื่อคืนค่าออกจากฟังก์ชันโดยตรง 
3. การละเว้น Semicolon ;: ท้ายโครงสร้าง match ไม่มีการใส่ ; เพื่อให้ผลลัพธ์ประเมินค่าเป็น Expression สำหรับคืนค่าให้ฟังก์ชัน check_user_role   
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

Rust แยก Expression และ Statement ออกจากกัน โดย Expression สามารถสร้างค่าได้ ส่วน Statement ใช้สำหรับจัดลำดับการทำงาน และ `;` มีผลต่อรูปแบบการใช้งานของ Expression  
ตัวอย่าง:
```rust
fn main() {
    let x = 10;  // Statement

    let y = x + 5;  // x + 5 เป็น Expression

    println!("{}", y);
}
```
ในตัวอย่าง `let x = 10;` เป็น Statement ส่วน `x + 5` เป็น Expression ที่สร้างค่า 15 เพื่อนำไปกำหนดให้ y

### 9.2 Semantics

Block ใน Rust สามารถเป็น Expression และมีค่าของตัวเองได้ โดย Expression ตัวสุดท้ายที่ไม่มี `;` จะเป็น Tail Expression และค่าของมันจะกลายเป็นค่าของ block หากมี ; ต่อท้าย ค่าจะถูกละทิ้งและ block จะได้ค่าเป็น ()  
ตัวอย่าง:
```rust
fn main() {
    let result = {
        let a = 10;
        let b = 20;

        a + b
    };

    println!("{}", result);
}
```
`a + b` เป็น Expression ตัวสุดท้ายของ block และไม่มี `;` ดังนั้นจึงเป็น Tail Expression และค่า 30 จะถูกส่งออกมาเป็นค่าของ block

### 9.3 Type System

เมื่อใช้ Expression เพื่อสร้างค่า Type ของค่าที่ได้ต้องสอดคล้องกัน โดยเฉพาะเมื่อใช้ if เป็น Expression  
ตัวอย่างที่ถูกต้อง:
```rust
fn main() {
    let score = 75;

    let grade = if score >= 70 {
        "Pass"
    } else {
        "Fail"
    };

    println!("{}", grade);
}
```
ทั้งสอง branch คืนค่าเป็น `&str` เหมือนกัน จึงสามารถนำผลลัพธ์ไปเก็บใน grade ได้

### 9.4 Memory / Resource Management

Block มี Scope ของตัวเอง ตัวแปรที่ประกาศภายใน block สามารถใช้ได้เฉพาะภายใน block นั้น  
ตัวอย่าง:
```rust
fn main() {
    let result = {
        let x = 10;
        x + 5
    };

    println!("{}", result);

    // println!("{}", x); // Error
}
```
ตัวแปร `x` สามารถใช้ภายใน `{ ... }` เท่านั้น แต่ `result` ได้รับค่า `15` จาก block และสามารถใช้ต่อด้านนอกได้

### 9.5 Abstraction / Other PPL Concepts

Rust มีแนวคิด `Expression-oriented programming` คือโครงสร้างหลายอย่างสามารถสร้างค่าได้ เช่น `if`, `loop` และ block  
ตัวอย่าง:
```rust
fn main() {
    let number = 10;

    let result = if number > 5 {
        100
    } else {
        0
    };

    println!("{}", result);
}
```
ในที่นี้ `if` ไม่ได้เป็นเพียงคำสั่งควบคุมการทำงาน แต่เป็น Expression ที่สร้างค่า `100` หรือ `0` แล้วนำไปเก็บใน `result`

### 9.6 Why Rust?

Rust ใช้แนวคิด `Expression-oriented programming` เพื่อให้โค้ดสามารถเขียนอย่างกระชับและมีโครงสร้างชัดเจน เช่น การใช้ block หรือ `if` เพื่อสร้างค่าโดยตรง รวมถึง `loop` ที่สามารถคืนค่าผ่าน `break value` ได้  
ตัวอย่าง loop ที่คืนค่า:
```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 3 {
            break counter * 10;
        }
    };

    println!("{}", result);
}
```

---

## 10. Rust vs. Other Language

**Comparison Language:** `[Python / C / C++ / Java / Kotlin / ...]`

| Aspect | Rust | Python |
|---|---|---|
| Syntax | `ใช้ {} สำหรับ Block และ ; ใช้แยก Statement` | `ใช้ indentation เพื่อกำหนด Block` |
| Semantics / Behavior | `if, loop และ Block สามารถเป็น Expression และคืนค่าได้` | `if และ loop ใช้ในลักษณะ Statement เป็นหลัก` |
| Type System | `Static Type System` | `Dynamic Type System` |
| Memory Management | `ใช้ Ownership และ Borrowing` | `จัดการ Memory อัตโนมัติ` |
| Safety | `Compiler ตรวจสอบ Type และกฎ Ownership/Borrowing` | `ตรวจสอบ Type หลัก ๆ ขณะ Runtime` |

### Rust Example

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
Output:
```rust
B
```
จุดสำคัญคือ `if` สามารถเป็น Expression และคืนค่า `"B"` ให้กับตัวแปร `grade` ได้

### `[Python]` Example

```python
score = 75

if score >= 80:
    grade = "A"
elif score >= 70:
    grade = "B"
else:
    grade = "C"

print(grade)
```
Output:
```python
B
```
ใน Python ต้องกำหนดค่าให้ `grade` ภายในแต่ละ branch ของ `if` ขณะที่ Rust สามารถใช้ `if` เป็น Expression แล้วกำหนดผลลัพธ์ให้ `grade` โดยตรง

### Analysis

ตัวอย่างนี้แสดงความแตกต่างด้านการออกแบบภาษาอย่างชัดเจน:
```rust
let grade = if score >= 70 {
    "B"
} else {
    "C"
};
```
`if` สร้างค่าออกมา แล้วนำค่านั้นไปกำหนดให้ `grade`  

Python:
```python
if score >= 70:
    grade = "B"
else:
    grade = "C"
```
`if` ทำหน้าที่ควบคุมการทำงาน และการกำหนดค่าให้ `grade` เกิดขึ้นภายในแต่ละ branch แสดงให้เห็นว่า Rust มีแนวทาง Expression-oriented ที่ทำให้โครงสร้างควบคุมสามารถนำมาใช้สร้างค่าได้โดยตรง

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
