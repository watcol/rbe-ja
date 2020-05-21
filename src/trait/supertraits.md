# 親トレイト

Rustは「継承(inheritance、deriveとは別)」を持っていませんが、トレイトを他のトレイトの
親として設定することができます。例えば

```rust,editable
trait Person {
    fn name(&self) -> String;
}

// StudentはPersonを親とする。
// Studentを実装するにはimpl Personが必要。
trait Student: Person {
    fn university(&self) -> String;
}

trait Programmer {
    fn fav_language(&self) -> String;
}

// CompSciStudent (コンピュータ科学の学生)はProgrammerとStudent両方を親とする。
// CompSciStudentの実装には両方の実装が必要。
trait CompSciStudent: Programmer + Student {
    fn git_username(&self) -> String;
}

fn comp_sci_student_greeting(student: &dyn CompSciStudent) -> String {
    format!(
        "My name is {} and I attend {}. My Git username is {}",
        student.name(),
        student.university(),
        student.git_username()
    )
}

fn main() {}
```

### こちらも参照:

[The Rust Programming Languageのsupertrait節の章][trpl_supertraits]

[trpl_supertraits]: https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#using-supertraits-to-require-one-traits-functionality-within-another-trait
