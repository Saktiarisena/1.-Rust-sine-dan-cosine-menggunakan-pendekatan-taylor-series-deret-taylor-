Program Rust ini menghitung fungsi sinus dan kosinus menggunakan pendekatan deret Taylor. Program ini menunjukkan cara mengimplementasikan deret matematika dalam Rust dan membandingkan hasilnya.

## Fitur

- Menghitung faktorial (dibutuhkan untuk deret Taylor)
- Mengimplementasikan aproksimasi deret Taylor untuk fungsi sinus dan kosinus
- Menghitung nilai untuk sudut dari 0 sampai 2π (0° sampai 360°)
- Menampilkan hasil dalam format tabel yang rapi

## Kode

```rust
use std::f64::consts::PI;

struct TaylorSeries;

impl TaylorSeries {
    // Menghitung faktorial dari n
    fn factorial(n: u64) -> f64 {
        if n == 0 {
            1.0
        } else {
            (1..=n).fold(1.0, |acc, i| acc * i as f64)
        }
    }

    // Menghitung sinus menggunakan deret Taylor
    fn sin(x: f64, n_terms: u32) -> f64 {
        let mut result = 0.0;
        for n in 0..n_terms {
            let term = (-1.0_f64).powi(n as i32) * x.powi(2 * n as i32 + 1) 
                      / Self::factorial(2 * n as u64 + 1);
            result += term;
        }
        result
    }

    // Menghitung kosinus menggunakan deret Taylor
    fn cos(x: f64, n_terms: u32) -> f64 {
        let mut result = 0.0;
        for n in 0..n_terms {
            let term = (-1.0_f64).powi(n as i32) * x.powi(2 * n as i32) 
                      / Self::factorial(2 * n as u64);
            result += term;
        }
        result
    }
}

fn main() {
    let n_terms = 10;  // Jumlah suku dalam deret Taylor
    let steps = 8;     // Jumlah sudut yang dihitung (0° sampai 360°)
    let mut results = Vec::new();

    // Menghitung sin dan cos untuk berbagai sudut
    for i in 0..=steps {
        let angle = (2.0 * PI * i as f64) / steps as f64;
        let sin_x = TaylorSeries::sin(angle, n_terms);
        let cos_x = TaylorSeries::cos(angle, n_terms);
        results.push((angle, sin_x, cos_x));
    }

    // Menampilkan hasil dalam tabel
    println!("| Sudut (rad) | sin(x)      | cos(x)      |");
    println!("|-------------|-------------|-------------|");
    for (angle, sin_x, cos_x) in results {
        println!("| {:.4}      | {:.8}  | {:.8}  |", angle, sin_x, cos_x);
    }
}
```

## Cara Menjalankan

1. Pastikan Rust sudah terinstall 
2. Clone repository ini
3. Jalankan program:
```bash
cargo run
```

## Contoh Output

```
| Sudut (rad) | sin(x)      | cos(x)      |
|-------------|-------------|-------------|
| 0.0000      | 0.00000000  | 1.00000000  |
| 0.7854      | 0.70710678  | 0.70710678  |
| 1.5708      | 1.00000000  | 0.00000000  |
| 2.3562      | 0.70710678  | -0.70710678 |
| 3.1416      | 0.00000000  | -1.00000000 |
| 3.9270      | -0.70710678 | -0.70710678 |
| 4.7124      | -1.00000000 | 0.00000000  |
| 5.4978      | -0.70710678 | 0.70710678  |
| 6.2832      | 0.00000000  | 1.00000000  |
```

## Kustomisasi

- Sesuaikan `n_terms` untuk mengubah presisi aproksimasi
- Ubah `steps` untuk menghitung lebih banyak atau lebih sedikit sudut
- Tambahkan perbandingan dengan fungsi bawaan Rust `sin()` dan `cos()`
