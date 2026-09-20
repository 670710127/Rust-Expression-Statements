## Challenge ที่อยากให้ทุกคนลองกลับไปหาข้อมูลกันดูนะครับ

## Challenge - วิเคราะห์ Expression ที่ซ้อนกันหลายระดับ

**Problem**
`โค้ดนี้ compile ผ่านหรือไม่ เพราะอะไร?`

```rust
let result = {
    let x = 4;
    let y = x + 2;

    if y > 5 {
        let z = {
            let a = y * 2;
            a - 3;
        };

        z + 1
    } else {
        y - 1
    }
};

println!("The result is: {}", result);
```